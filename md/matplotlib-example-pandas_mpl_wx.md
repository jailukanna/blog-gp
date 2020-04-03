---
title: matplotlib example pandas mpl wx (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'pandas mpl wx'


Modules used in program: 
* `import pandas as pd`
* `import wx`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import re`
* `import argparse`
* `import math`

## python pandas mpl wx

Python matplotlib example: pandas mpl wx

```python
import math
import argparse
import re

from numpy import arange, sin, pi
import matplotlib
import matplotlib.pyplot as plt
matplotlib.use('WXAgg')

from matplotlib.backends.backend_wxagg import FigureCanvasWxAgg as FigureCanvas
from matplotlib.backends.backend_wx import NavigationToolbar2Wx
from matplotlib.figure import Figure

import wx
import pandas as pd
from pandas import Series, DataFrame

class CanvasPanel(wx.Panel):
    def __init__(self, parent):
        wx.Panel.__init__(self, parent)
        self.slider = wx.Slider(self, -1, value=1, minValue=0, maxValue=11)
        self.slider.Bind(wx.EVT_SLIDER, self.OnSlider)
        self.figure = Figure()
        self.axes = self.figure.add_subplot(111)
        self.canvas = FigureCanvas(self, -1, self.figure)
        self.toolbar = NavigationToolbar2Wx(self.canvas)
        self.toolbar.AddLabelTool(5,'',wx.ArtProvider.GetBitmap(wx.ART_FILE_OPEN, wx.ART_TOOLBAR, (32,32)))
        self.toolbar.Realize()      
        
        self.sizer = wx.BoxSizer(wx.VERTICAL)
        self.sizer.Add(self.toolbar, 0, wx.LEFT | wx.TOP | wx.GROW)
        self.sizer.Add(self.canvas, proportion=1, 
                        flag=wx.LEFT | wx.TOP | wx.GROW)
        self.sizer.Add(self.slider, proportion=0, 
                        flag=wx.LEFT | wx.TOP | wx.GROW)
        self.SetSizer(self.sizer)
        self.Fit()
        self.get_data()
        box = self.axes.get_position()
        self.axes.set_position([box.x0, box.y0+box.height*0.25, box.width, box.height*0.75])


    def resample(self, df):
        # How do I do this smartly
        dfg = df.reset_index().set_index('time_stamp').groupby('pnode')
        pnodes = {}
        for pnode in dfg.groups.keys():
            #print("Resampling: %s" % (pnode))
            pnodes[pnode] = df.ix[pnode].resample('M', how='mean')

        df2 = pd.concat(pnodes, names=['pnode'])
        return df2

    def get_units(self, store):
        if '/df' in store.keys():
            print("df key already exists")
            df = store['df']
            return df

        lip = store.select('lip')
        pnodes = lip.groupby(level=0).first()

        nodes = []
        pattern = r'CSWS\w+UN\d+'
        # there are about 50 units
        for node in pnodes.index:
            if re.match(pattern, node):
                nodes.append(node)

        # Retrieval is slow.  We are fighting 32bit limits so we can't bring
        # everything into memory.  About 6 seconds per node.
        # Term() conditions can only be ANDed together.
        df = store.select('lip', pd.Term("pnode==CSWS_LA"))
        for node in nodes:
            print("Getting node: %s" % node)
            df = df.append(store.select('lip', pd.Term("pnode", "=", node)))
        # Save it here for later
        df = df.append(store.select('lip', pd.Term("pnode", "=", "CSWS_LA")))
        store['df'] = df
        return df

    def get_data(self):
        store = pd.HDFStore('spp.h5') 
        df = self.get_units(store)
        store.close()
        self.dfs = self.resample(df)
        self.nodes = self.dfs.index.get_level_values('pnode').unique()

    def plot_month(self):
        ax = self.axes
        ax.clear()
        nodes = self.nodes
        dfs = self.dfs
        month_num = self.slider.GetValue()
        times = dfs.index.get_level_values('time_stamp')
        mon = dfs.xs(times[month_num], level='time_stamp')
        mon.plot(ax=self.axes, style='o')

        ax.set_title('Month Num: %s' % month_num)
        ax.axhline(mon.ix["CSWS_LA"], color='k', linewidth=2)
        x = range(len(nodes))
        ax.set_xticks(x)
        ax.set_xticklabels(nodes)
        ys = ax.get_yticks()
        ys = [math.floor(y) for y in ys]
        ys.append(ys[-1] + 1)
        ax.set_yticks(ys)

        for tick in ax.xaxis.get_major_ticks():
                tick.label.set_fontsize('small') 
                tick.label.set_rotation('vertical')
        
        ax.grid()
        self.canvas.draw()

    def OnSlider(self, event):
        self.plot_month()

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
    panel.plot_month()
    frame.Show()
    app.MainLoop()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
