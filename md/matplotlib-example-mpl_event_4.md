---
title: matplotlib example mpl event 4 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mpl event 4'

Functions in program: 
* `def leave_figure(event):`
* `def enter_figure(event):`
* `def leave_axes(event):`
* `def enter_axes(event):`

Modules used in program: 
* `import ipywidgets`
* `import matplotlib`
* `import matplotlib.pyplot as plt`

## python mpl event 4

Python matplotlib example: mpl event 4

```python
#jupyterlab
%matplotlib widget
#jupyter notebook
#%matplotlib notebook
#GUI
#%matplotlib

import matplotlib.pyplot as plt
from ipywidgets import Output

out = Output()
display(out)
@out.capture(clear_output=True)

def enter_axes(event):
    print('enter_axes', event.inaxes)
    event.inaxes.patch.set_facecolor('g')
    event.canvas.draw()

def leave_axes(event):
    print('leave_axes', event.inaxes)
    event.inaxes.patch.set_facecolor('white')
    event.canvas.draw()

def enter_figure(event):
    print('enter_figure', event.canvas.figure)
    event.canvas.figure.patch.set_facecolor('lightgreen')
    event.canvas.draw()

def leave_figure(event):
    print('leave_figure', event.canvas.figure)
    event.canvas.figure.patch.set_facecolor('pink')
    event.canvas.draw()

#show
fig,ax = plt.subplots(ncols=2,nrows=2)
fig.suptitle('mouse hover over figure or axes to trigger events')
ax = ax.ravel()
[ax[i].set(xticklabels=[],yticklabels=[]) for i in range(4)]

fig.canvas.mpl_connect('figure_enter_event', enter_figure)
fig.canvas.mpl_connect('figure_leave_event', leave_figure)
fig.canvas.mpl_connect('axes_enter_event', enter_axes)
fig.canvas.mpl_connect('axes_leave_event', leave_axes)
plt.show()

import matplotlib
print(matplotlib.__version__)
#3.2.0
 
import ipywidgets
print(ipywidgets.__version__)
#7.5.1

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
