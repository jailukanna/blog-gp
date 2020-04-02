---
title: pil example matplotlibTemplate (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'matplotlibTemplate'

Functions in program: 
* `def touch(ax, data):`

Modules used in program: 
* `import matplotlib as mpl`
* `import numpy as np`

## python matplotlibTemplate

Python pil example: matplotlibTemplate

```python
"""
Author: Miladious
Latest Modification Date: Oct 11, 2019
About: 
Creating a publication quality figure using matplotlib requires a lot of tweaks.
In this gist, I show the main tweaks for creating publication quality plots. 
This is an example how one would plot astronomical images in python. 
Feel free to modify to your field's standards.
"""

# Import Image tools
import numpy as np
from PIL import Image
from urllib.request import urlopen
from IPython.display import display

# Import plotting tools
from matplotlib import pyplot as plt
import matplotlib as mpl

# Load a PNG image as a PIL image
img1 = Image.open(urlopen('https://www.gstatic.com/webp/gallery3/3.png'))
# Convert HWC PIL image to CHW numpy array
img1 = np.asanyarray(img1)
# Create a list with 12 copies of the same loaded image
dataList = [img1 for i in range(12)]

# Define master color
mc1 = '6e6558'

with plt.rc_context({
        'figure.facecolor' : 'w',
        
        'axes.facecolor'   : 'w',
        'axes.edgecolor'   : mc1, 
        'axes.linewidth'   : 2,

        'xtick.color'      : mc1, 
        'ytick.color'      : mc1,

    }):

    # Figure and axes setup
    fig, axs = plt.subplots(nrows=3, ncols=4, figsize = (8,6), dpi=200)
    fig.subplots_adjust(wspace=-.0, hspace=-.5)

    
def touch(ax, data):
    """
    touch will modify each ax with the goal to prevent repetition.
    one can define a new touch function for axs that must be modified differently.
    """
    
    # Plot data
    ax.imshow(data, origin='lower',interpolation='nearest', alpha=.99, aspect='equal')

    
    # Set axis range
    #ax.set_xlim(0.0,63.)
    #ax.set_ylim(0.0,63.)

    
    # Make these tick labels invisible
    plt.setp(ax.get_xticklabels(), visible=False)
    plt.setp(ax.get_yticklabels(), visible=False)


    # Customize the tick marks and turn the grid on
    ax.xaxis.set_ticks_position('both')
    ax.yaxis.set_ticks_position('both')
    ax.minorticks_on()
    ax.tick_params(which='major', length=7, width=1.5, direction='in', pad=5)
    ax.tick_params(which='minor', length=3, width=1.0, direction='in', pad=5)
    ax.grid(which='both', alpha=.0)
    
    ax.set_aspect('equal')

    return

# if only one ax
if str(type(axs)) == "<class 'matplotlib.axes._subplots.AxesSubplot'>":
    touch(axs, dataList[0])
# if a grid of axes
else:
    for i, ax in enumerate(axs.reshape(-1)):
        touch(ax, dataList[i])


plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
