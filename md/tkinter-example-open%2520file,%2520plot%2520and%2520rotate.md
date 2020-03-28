---
title: tkinter example open%2520file,%2520plot%2520and%2520rotate (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'open%2520file,%2520plot%2520and%2520rotate'

Functions in program: 
* `def rotate_vector(v, angle, anchor):`

Modules used in program: 
* `import math`
* `import csv`
* `import os`
* `import tkFileDialog`
* `import Tkinter as Tk`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python open%2520file,%2520plot%2520and%2520rotate

Python tkinter example: open%2520file,%2520plot%2520and%2520rotate

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.widgets import Button
import Tkinter as Tk
import tkFileDialog
import os
import csv
import math

freqs = np.arange(2, 20, 3)

fig, ax = plt.subplots()
plt.subplots_adjust(bottom=0.2)
t = 0
s = 0
l, = plt.plot(t, s, lw=2)
plt.axis([-1, 30, -5,2])
x=[]
z=[]
x_rotate=[]
z_rotate=[]

def rotate_vector(v, angle, anchor):
    """Rotate a vector `v` by the given angle, relative to the anchor point."""
    x, y = v

    x = float(x) - anchor[0]
    y = float(y) - anchor[1]
    # Here is a compiler optimization; inplace operators are slower than
    # non-inplace operators like above. This function gets used a lot, so
    # performance is critical.

    cos_theta = math.cos(math.radians(angle))
    sin_theta = math.sin(math.radians(angle))

    nx = x*cos_theta - y*sin_theta
    ny = x*sin_theta + y*cos_theta

    nx = nx + anchor[0]
    ny = ny + anchor[1]
    return [nx, ny]

class Index(object):
    ind = 0

    def open(self, event):
         folder = tkFileDialog.askopenfilename() # show an "Open" dialog box and return the path to the selected file
        
         f= open(folder)
        #append values to list
        for row in csv.reader(f):
            x.append(row[0])
            z.append(row[1])
            angle= 8
            x_rotate.append(rotate_vector((row[0],row[1]),angle,(0,0))[0])
            # rotate x to 8 angle 
            z_rotate.append(rotate_vector((row[0],row[1]),angle,(0,0))[1])
            # rotate z to 8 angle             
        
          l.set_xdata(x)
          l.set_ydata(z)
          plt.draw()

    def align(self, event):
         plt.clf()
         plt.plot(x_rotate, z_rotate, 'r.')
         plt.axis([-1, 30, -5,2])
         plt.draw()


callback = Index()
axopen = plt.axes([0.7, 0.05, 0.1, 0.075])
bopen = Button(axopen, 'Open')
bopen.on_clicked(callback.open)
axalign = plt.axes([0.81, 0.05, 0.1, 0.075])
balign = Button(axalign, 'Align')
balign.on_clicked(callback.align)

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
