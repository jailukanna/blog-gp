---
title: matplotlib example mpl wxpython example (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mpl wxpython example'


Modules used in program: 
* `import wx`
* `import matplotlib`
* `import argparse`

## python mpl wxpython example

Python matplotlib example: mpl wxpython example

```python
import argparse

from numpy import arange, sin, pi
import matplotlib
matplotlib.use('WXAgg')

from matplotlib.backends.backend_wxagg import FigureCanvasWxAgg as FigureCanvas
from matplotlib.backends.backend_wx import NavigationToolbar2Wx
from matplotlib.figure import Figure

import wx

class CanvasPanel(wx.Panel):
    def __init__(self, parent):
        wx.Panel.__init__(self, parent)
        self.slider = wx.Slider(self, -1, value=1, minValue=0, maxValue=11)
        self.slider.Bind(wx.EVT_SLIDER, self.OnSlider)
        self.figure = Figure()
        self.axes = self.figure.add_subplot(111)
        self.canvas = FigureCanvas(self, -1, self.figure)
        self.sizer = wx.BoxSizer(wx.VERTICAL)
        self.sizer.Add(self.canvas, proportion=1, 
                        flag=wx.LEFT | wx.TOP | wx.GROW)
        self.sizer.Add(self.slider, proportion=0, 
                        flag=wx.LEFT | wx.TOP | wx.GROW)
        self.SetSizer(self.sizer)
        self.Fit()

    def OnSlider(self, event):
        self.draw()

    def draw(self):
        t = arange(0.0, 3.0, 0.01)
        h = self.slider.GetValue()
        s = h * sin(2*pi*t)
        self.axes.clear()
        self.axes.plot(t, s)
        self.canvas.draw()
        

if __name__ == "__main__":
    app = wx.PySimpleApp()
    frame = wx.Frame(None, title='test')
    panel = CanvasPanel(frame)
    panel.draw()
    frame.Show()
    app.MainLoop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
