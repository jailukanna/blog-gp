---
title: matplotlib example kivy spectrogram (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'kivy spectrogram'

Functions in program: 
* `def figure_leave(event):`
* `def figure_enter(event):`
* `def axes_leave(event):`
* `def axes_enter(event):`
* `def close(event):`
* `def scroll(event):`
* `def resize(event):`
* `def motionnotify(event):`
* `def keyup(event):`
* `def keypress(event):`
* `def release(event):`
* `def press(event):`

Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib`

## python kivy spectrogram

Python matplotlib example: kivy spectrogram

```python
#!/usr/bin/env python2.7
# Kivy + Matplotlib + Scipy spectrogram test
# Julien Deudon (initbrain)
# 201703160114

# See :
# https://github.com/kivy-garden/garden.matplotlib/tree/master/examples
# https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.spectrogram.html


import matplotlib
matplotlib.use('module://kivy.garden.matplotlib.backend_kivy')

import matplotlib.pyplot as plt
import numpy as np
from scipy import signal

from kivy.app import App
from kivy.lang import Builder
from kivy.uix.boxlayout import BoxLayout
from kivy.garden.matplotlib.backend_kivyagg import FigureCanvas


def press(event):
    print(('press released from test', event.x, event.y, event.button))


def release(event):
    print(('release released from test', event.x, event.y, event.button))


def keypress(event):
    print(('key down', event.key))


def keyup(event):
    print(('key up', event.key))


def motionnotify(event):
    print(('mouse move to ', event.x, event.y))


def resize(event):
    print(('resize from mpl ', event))


def scroll(event):
    print(('scroll event from mpl ', event.x, event.y, event.step))


def close(event):
    print(('closing figure'))


def axes_enter(event):
    print(('enter_axes', event.inaxes))
    event.inaxes.patch.set_facecolor('blue')
    event.canvas.draw()


def axes_leave(event):
    print(('leave_axes', event.inaxes))
    event.inaxes.patch.set_facecolor('black')
    event.canvas.draw()


def figure_enter(event):
    print(('enter_figure', event.canvas.figure))
    event.canvas.figure.patch.set_facecolor('darkred')
    event.canvas.draw()


def figure_leave(event):
    print(('leave_figure', event.canvas.figure))
    event.canvas.figure.patch.set_facecolor('black')
    event.canvas.draw()
    print(event.canvas.figure)


kv = """
<Test>:
    orientation: 'vertical'
    Button:
        size_hint_y: None
        height: 40
"""

Builder.load_string(kv)


class Test(BoxLayout):
    def __init__(self, *args, **kwargs):
        super(Test, self).__init__(*args, **kwargs)
        self.add_plot()

    def get_fc(self, i):
        #print(plt.style.available)
        with plt.style.context(('dark_background')):
            figure = plt.figure()

            figure.suptitle('mouse hover over figure or axes to trigger events' +
                          str(i))
            ax1 = figure.add_subplot(211)
            ax2 = figure.add_subplot(212)
            wid = FigureCanvas(figure)
            self.add_bindings(figure)
        return wid

    def get_sinusoide(self):
        # Generate a test signal, a 2 Vrms sine wave whose frequency is slowly
        # modulated around 3kHz, corrupted by white noise of exponentially
        # decreasing magnitude sampled at 10 kHz.
        fs = 10e3
        N = 1e5
        amp = 2 * np.sqrt(2)
        noise_power = 0.01 * fs / 2
        time = np.arange(N) / float(fs)
        mod = 500*np.cos(2*np.pi*0.25*time)
        carrier = amp * np.sin(2*np.pi*3e3*time + mod)
        noise = np.random.normal(scale=np.sqrt(noise_power), size=time.shape)
        noise *= np.exp(-time/5)
        x = carrier + noise
        return x, fs

    def get_spectrogram(self, x, fs):
        #print(plt.style.available)
        with plt.style.context(('dark_background')):
            figure = plt.figure()
            #figure.set_facecolor('gray')
            #figure.patch.set_alpha(0.3)

            # Compute and plot the spectrogram.
            f, t, Sxx = signal.spectrogram(x, fs)
            plt.ylabel('Frequency [Hz]')
            plt.xlabel('Time [sec]')
            plt.pcolormesh(t, f, Sxx)
            plt.show()
            wid = FigureCanvas(figure)
            self.add_bindings(figure)
        return wid

    def add_plot(self):
        self.add_widget(self.get_fc(1))
        #self.add_widget(self.get_fc(2))
        x, fs = self.get_sinusoide()
        self.add_widget(self.get_spectrogram(x, fs))

    def add_bindings(self, figure):
        figure.canvas.mpl_connect('button_press_event', press)
        figure.canvas.mpl_connect('button_release_event', release)
        figure.canvas.mpl_connect('key_press_event', keypress)
        figure.canvas.mpl_connect('key_release_event', keyup)
        figure.canvas.mpl_connect('motion_notify_event', motionnotify)
        figure.canvas.mpl_connect('resize_event', resize)
        figure.canvas.mpl_connect('scroll_event', scroll)
        figure.canvas.mpl_connect('figure_enter_event', figure_enter)
        figure.canvas.mpl_connect('figure_leave_event', figure_leave)
        figure.canvas.mpl_connect('axes_enter_event', axes_enter)
        figure.canvas.mpl_connect('axes_leave_event', axes_leave)
        figure.canvas.mpl_connect('close_event', close)


class TestApp(App):
    def build(self):
        return Test()

if __name__ == '__main__':
    TestApp().run()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
