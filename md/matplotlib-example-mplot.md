---
title: matplotlib example mplot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mplot'


Modules used in program: 
* `import matplotlib`
* `import matplotlib.pyplot as plt`
* `import tkinter as tk `

## python mplot

Python matplotlib example: mplot

```python
import tkinter as tk 
from tkinter import ttk
import matplotlib.pyplot as plt
import matplotlib
matplotlib.use("TkAgg")
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg, NavigationToolbar2TkAgg
from matplotlib.figure import Figure

win = tk.Tk()
#plt.plot([1,2,3,4])
#plt.ylabel('some numbers')
frame = ttk.Frame(win)
f = Figure(figsize=(5,5), dpi=100)
a = f.add_subplot(111)
a.plot([1,2,3,4,5,6,7,8],[5,6,1,3,8,9,3,5])

canvas = FigureCanvasTkAgg(f, frame)
canvas.show()
canvas.get_tk_widget().pack(side=tk.BOTTOM, fill=tk.BOTH, expand=True)
frame.pack()
win.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
