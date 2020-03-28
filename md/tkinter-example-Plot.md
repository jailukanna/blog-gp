---
title: tkinter example Plot (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'Plot'


Modules used in program: 
* `import tkinter.ttk as ttk`
* `import tkinter as Tk`
* `import matplotlib`

## python Plot

Python tkinter example: Plot

```python
import matplotlib
matplotlib.use('TkAgg')
from numpy import arange, sin, pi
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg, NavigationToolbar2TkAgg
from matplotlib.figure import Figure
import tkinter as Tk
import tkinter.ttk as ttk

class Plot2D(Tk.Frame):
    def __init__(self,parent,**kwargs):
        Tk.Frame.__init__(self,parent,**kwargs)

        self.traces = dict()

        # Define matplotlib figure
        self.f = Figure(figsize=(5,4), dpi=100)
        self.a = self.f.add_subplot(111)

        # Tell Tkinter to display matplotlib figure
        self.canvas = FigureCanvasTkAgg(self.f, master=parent)
        self.canvas.show()
        self.canvas.get_tk_widget().pack(side=Tk.TOP, fill=Tk.BOTH, expand=1)


    def trace(self,name,dataset_x,dataset_y):
        if name in self.traces:
            self.line1.set_data(dataset_x,dataset_y)
        else:
            self.traces[name] = True
            self.line1, = self.a.plot(dataset_x,dataset_y)
        self.canvas.draw()


if __name__=="__main__":
    root = Tk.Tk()
    plot = Plot2D(root)

    phase = 0

    # Function executed every 50 ms
    def up(phase):
        t = arange(0.0, 3.0, 0.01)
        s = sin(2*pi*t + phase)
        phase += 0.1

        plot.trace("test",t,s)
        root.after(50,up,phase)

    up(phase)

    root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
