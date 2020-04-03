---
title: matplotlib example matplotlib in tkinter (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib in tkinter'


Modules used in program: 
* `import tkinter as tk`
* `import matplotlib`

## python matplotlib in tkinter

Python matplotlib example: matplotlib in tkinter

```python

import matplotlib
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg
from matplotlib.backends.backend_tkagg import NavigationToolbar2TkAgg
from numpy import arange, sin, pi
import tkinter as tk

matplotlib.use('TkAgg')


root = tk.Tk()
root.wm_title("Embedding in TK")


figure = matplotlib.figure.Figure(figsize=(5, 4), dpi=100)
sub_plt = figure.add_subplot(111)
v_time = arange(0.0, 3.0, 0.01)
v_sine = sin(2*pi*v_time)

sub_plt.plot(v_time, v_sine)

canvas = FigureCanvasTkAgg(figure, master=root)
canvas.show()
canvas.get_tk_widget().pack(side='top', fill='both', expand=1)

toolbar = NavigationToolbar2TkAgg(canvas, root)
toolbar.update()
canvas._tkcanvas.pack(side='top', fill='both', expand=1)

root.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
