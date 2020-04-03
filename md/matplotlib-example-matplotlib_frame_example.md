---
title: matplotlib example matplotlib frame example (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib frame example'

Functions in program: 
* `def change_data():`

Modules used in program: 
* `import matplotlib_frame`
* `import tkinter as Tk`

## python matplotlib frame example

Python matplotlib example: matplotlib frame example

```python
# An example showing how to use the GraphFrame class

import tkinter as Tk
import matplotlib_frame
from scipy import sin, cos, linspace, pi

# Create the root window
root = Tk.Tk()

# Create a GraphFrame and pack it inside of root
gf = matplotlib_frame.GraphFrame(root, show_toolbar=True)
gf.pack(side=Tk.TOP)

# Plot two functions
space = linspace(0, 4*pi, 100)
gf.subplot.plot(space, cos(space))
line, = gf.subplot.plot(space, sin(space))

# Add a button for changing data
def change_data():
    line.set_data(space, cos(space+pi/4))
    # Note that the canvas needs to be redrawn after a data change
    gf.draw()
b = Tk.Button(root, text="Change data", command=change_data)
b.pack()

# Start the mainloop
root.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
