---
title: matplotlib example Beautify (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Beautify'

Functions in program: 
* `def Beautify(xlabel=None, ylabel=None, title=None, legendstrs=None, savefile=None, boldlevel=1, figax=None):`

Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib`

## python Beautify

Python matplotlib example: Beautify

```python
# Created 2014, Zack Gainsforth
import matplotlib
matplotlib.use('Qt4Agg')
import matplotlib.pyplot as plt
import numpy as np

def Beautify(xlabel=None, ylabel=None, title=None, legendstrs=None, savefile=None, boldlevel=1, figax=None):
    """
    :param xlabel: String (can include TeX) for the label of the x-axis.
    :param ylabel: String (can include TeX) for the label of the y-axis.
    :param title: String (can include TeX) for the label of the title.
    :param legendstrs: List of strings with the same length as x and y, axis 1.
    :param savefile: A filename to save to if desired.
    :param boldlevel: Sets the linewidths fatter and texts bigger.  1 is normal, 4 is usually good for publication.
    :param figax: Apply to this figure and axis supplied as tuple (fig, ax).  Or gets the gcf and gca.
    :return: (fig, ax) The figure and axis so the user can do stuff of his own with it if he wants to.
    """
 
    # Set the bold level.
    FontSizeBasis = (boldlevel+2)*4    # Fonts get bigger as boldlevel increases
    TickMajorBasis = boldlevel*4    # As fonts get bigger, they need a larger padding from the axis.
    # Increase the size of the tick label fonts.
    matplotlib.rc('xtick', labelsize=FontSizeBasis)
    matplotlib.rc('ytick', labelsize=FontSizeBasis)
    # Increase their padding.
    matplotlib.rc('xtick.major', pad=TickMajorBasis)
    matplotlib.rc('ytick.major', pad=TickMajorBasis)
    matplotlib.rc('lines', linewidth=boldlevel)
    
    # Create/reuse a figure to plot on.
    if figax is None:
        fig, ax = plt.gcf(), plt.gca()        
 
    # Write the x and y labels and the title.
    if xlabel is not None:
        plt.xlabel(xlabel, fontsize=FontSizeBasis)
    if ylabel is not None:
        plt.ylabel(ylabel, fontsize=FontSizeBasis)
    if title is not None:
        plt.title(title, fontsize=FontSizeBasis)
 
    # Show a legend if input.
    if legendstrs is not None:
        ax.legend(legendstrs)
 
    # Resize the figure appropriately
    plt.tight_layout()
    
    # Update the figure.
    plt.draw()
 
    # Save it to a file if a name is given.
    if savefile is not None:
        plt.savefig(savefile)
 
    return fig, ax



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
