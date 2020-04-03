---
title: matplotlib example strip chart demo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'strip chart demo'

Functions in program: 
* `def emitter(p=0.03):`

Modules used in program: 
* `import matplotlib.animation as animation`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import kivy.garden.matplotlib.backend_kivy`
* `import matplotlib`

## python strip chart demo

Python matplotlib example: strip chart demo

```python
"""
Emulate an oscilloscope.  Requires the animation API introduced in
matplotlib 1.0 SVN.
"""
from kivy.app import App
from kivy.uix.widget import Widget
from kivy.uix.boxlayout import BoxLayout

import matplotlib
matplotlib.use("module://kivy.garden.matplotlib.backend_kivy")
import kivy.garden.matplotlib.backend_kivy
from kivy.garden.matplotlib import FigureCanvasKivy, FigureCanvasKivyAgg
from matplotlib.backends.backend_agg import FigureCanvasAgg

import numpy as np
from matplotlib.lines import Line2D
import matplotlib.pyplot as plt
import matplotlib.animation as animation


class Scope(object):
    def __init__(self, ax, maxt=2, dt=0.02):
        self.ax = ax
        self.dt = dt
        self.maxt = maxt
        self.tdata = [0]
        self.ydata = [0]
        self.line = Line2D(self.tdata, self.ydata)
        self.ax.add_line(self.line)
        self.ax.set_ylim(-.1, 1.1)
        self.ax.set_xlim(0, self.maxt)

    def update(self, y):
        lastt = self.tdata[-1]
        if lastt > self.tdata[0] + self.maxt:  # reset the arrays
            self.tdata = [self.tdata[-1]]
            self.ydata = [self.ydata[-1]]
            self.ax.set_xlim(self.tdata[0], self.tdata[0] + self.maxt)
            self.ax.figure.canvas.draw()

        t = self.tdata[-1] + self.dt
        self.tdata.append(t)
        self.ydata.append(y)
        self.line.set_data(self.tdata, self.ydata)
        return self.line,


def emitter(p=0.03):
    'return a random value with probability p, else 0'
    while True:
        v = np.random.rand(1)
        if v > p:
            yield 0.
        else:
            yield np.random.rand(1)


class MyApp(App):

    def build(self):
        return Widget()

    def on_start(self, *args):

        fig, ax = plt.subplots()
        scope = Scope(ax)

        # pass a generator in "emitter" to produce data for the update func
        ani = animation.FuncAnimation(fig, scope.update, emitter, interval=10,
                                      blit=True)
        plt.show()


if __name__ == "__main__":
    app = MyApp()
    app.run()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
