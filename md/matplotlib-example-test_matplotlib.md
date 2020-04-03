---
title: matplotlib example test matplotlib (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'test matplotlib'

Functions in program: 
* `def reset(event):`
* `def update(val):`

Modules used in program: 
* `import numpy as np`

## python test matplotlib

Python matplotlib example: test matplotlib

```python
try:
    import matplotlib
    import matplotlib.pyplot as plt
except ImportError:
    gui_env = ['TKAgg','GTKAgg','Qt4Agg','WXAgg']
    for gui in gui_env:
        try:
            matplotlib.use(gui,warn=False, force=True)
            from matplotlib import pyplot as plt
            break
        except:
            continue

import numpy as np
from matplotlib.widgets import Slider, Button, RadioButtons

fig, ax = plt.subplots()
plt.subplots_adjust(left=0.1, bottom=0.33)
t = np.arange(0.0, 1.0, 0.001)
a0 = 0.5
f0 = 3
p0 = 0
s = a0*np.sin(2*np.pi*f0*t+p0)
l, = plt.plot(t, s, lw=2, color='red')
plt.axis([0, 1, -1.1, 1.1])

axcolor = 'lightgoldenrodyellow'
axfreq = plt.axes([0.1, 0.2, 0.8, 0.03], facecolor=axcolor)
axamp = plt.axes([0.1, 0.15, 0.8, 0.03], facecolor=axcolor)
axphase = plt.axes([0.1, 0.1, 0.8, 0.03], facecolor=axcolor)

sfreq = Slider(axfreq, 'Freq', 0.1, 30.0, valinit=f0)
samp = Slider(axamp, 'Amp', 0.1, 1, valinit=a0)
sphase = Slider(axphase, 'Phase', -np.pi, np.pi, valinit=p0)

def update(val):
    amp = samp.val
    freq = sfreq.val
    phase = sphase.val
    l.set_ydata(amp*np.sin(2*np.pi*freq*t + phase))
    fig.canvas.draw_idle()

sfreq.on_changed(update)
samp.on_changed(update)
sphase.on_changed(update)

resetax = plt.axes([0.8, 0.025, 0.1, 0.04])
button = Button(resetax, 'Reset', color=axcolor, hovercolor='0.975')


def reset(event):
    sfreq.reset()
    samp.reset()
    sphase.reset()
button.on_clicked(reset)

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
