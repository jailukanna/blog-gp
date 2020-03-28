---
title: tkinter example plotLogfile (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'plotLogfile'


Modules used in program: 
* `import tkFileDialog`
* `import sys`
* `import matplotlib.cm as cm`
* `import matplotlib.dates as mdates`
* `import matplotlib.animation as animation`
* `import matplotlib`
* `import os`
* `import random`
* `import numpy as np`

## python plotLogfile

Python tkinter example: plotLogfile

```python
# Plot log files
# version 3
# Amar 
# Changes: v2: 2012/11/27: Date & time display using native matplotlib, instead of unix timestamp
#          v3: 2013/10/10: Rewritten completely using matplotlib animation, and slider for selectable range

import numpy as np
import random
import os
os.chdir("c:/data/googledrive/logs")
from datetime import datetime

import matplotlib
import matplotlib.animation as animation
from matplotlib.widgets import Slider, Button, RadioButtons
import matplotlib.dates as mdates
import matplotlib.cm as cm
matplotlib.use('TkAgg')
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg, NavigationToolbar2TkAgg
from matplotlib.figure import Figure

import sys
if sys.version_info[0] < 3:
    import Tkinter as Tk
else:
    import tkinter as Tk
import tkFileDialog

if __name__ == '__main__':
    root = Tk.Tk()
    try:
        filename = tkFileDialog.askopenfilename(parent=root,title='Choose file to plot')
        root.wm_title(filename)

        fig = Figure()
        ax = fig.add_subplot(111)
        ax.autoscale(enable=True)
        canvas = FigureCanvasTkAgg(fig, master=root)
        canvas.show()
        canvas.get_tk_widget().pack(side=Tk.TOP, fill=Tk.BOTH, expand=1)
        toolbar = NavigationToolbar2TkAgg( canvas, root )
        toolbar.update()
        canvas._tkcanvas.pack(side=Tk.TOP, fill=Tk.BOTH, expand=1)

        # For showing dates
        fig.autofmt_xdate()
        ax.fmt_xdata = mdates.DateFormatter('%Y-%m-%d %H:%M:%S')

        # Set titles and comments
        f = file(filename,'r')
        titleLine = f.readline()[2:]  # first line is the log comment 
        commentLine = f.readline()    # second line contains the column labels
        commentLine = commentLine[1:-1]
        comments = commentLine.split("\t")
        f.close()
        compactFileName = filename.split('/')[-1]
        ax.set_title(titleLine) 
        ax.set_ylabel(comments[1])

        axcolor = 'lightgoldenrodyellow'
        axNumber = fig.add_axes([0.25, 0.2, 0.65, 0.03],axisbg=axcolor)
        sNumber = Slider(axNumber, '# of pts', 1000, 10000, valfmt='%0.0f', valinit=5000)
        sNumber.valtext.set_visible(False)
        #sNumber.on_changed(duh)

        x0,y0 = np.loadtxt(filename,unpack=True)
        x0,y0 = x0[-sNumber.val::5],y0[-sNumber.val::5]
        x0 = map(datetime.fromtimestamp,x0)
        line, = ax.plot(x0,y0,linewidth=1.2,color = cm.spectral(random.randrange(0,255)))

        def update(dummyVar):
            try:
                x,y = np.loadtxt(filename,unpack=True) #np.random.rand(10),np.random.rand(10)
                x1,y1 = x[-sNumber.val::5],y[-sNumber.val::5]    
                yrange = max(y1) - min(y1)
                yavg = (max(y1) + min(y1))/2.
                ylims = yavg - 1.05*yrange/2., yavg + 1.05*yrange/2.
                                
                ax.set_ylim(ylims[0],ylims[1])
                x1 = map(datetime.fromtimestamp,x1)
                ax.set_xlim(min(x1),max(x1))
                line.set_xdata(x1)
                line.set_ydata(y1)
            
            except ValueError or UnboundLocalError: pass
            return line,

        def data_gen():
            while True: yield True

        ani = animation.FuncAnimation(fig, update, data_gen, interval=3000)

    except IOError: pass

    Tk.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
