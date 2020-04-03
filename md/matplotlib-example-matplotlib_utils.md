---
title: matplotlib example matplotlib utils (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib utils'

Functions in program: 
* `def colorbar(mappable, ticks):`
* `def discrete_cmap(N, base_cmap=None):`

Modules used in program: 
* `import matplotlib.pyplot as plt`

## python matplotlib utils

Python matplotlib example: matplotlib utils

```python
"""Inspired by:
https://gist.github.com/jakevdp/91077b0cae40f8f8244a
http://joseph-long.com/writing/colorbars/
"""

import matplotlib.pyplot as plt
from mpl_toolkits.axes_grid1 import make_axes_locatable

def discrete_cmap(N, base_cmap=None):
    """Create an N-bin discrete colormap from the specified input map"""
    base = plt.cm.get_cmap(base_cmap)
    color_list = base(np.linspace(0, 1, N))
    cmap_name = base.name + str(N)
    return base.from_list(cmap_name, color_list, N)

def colorbar(mappable, ticks):
    ax = mappable.axes
    fig = ax.figure
    divider = make_axes_locatable(ax)
    cax = divider.append_axes("right", size="5%", pad=0.05)
    return fig.colorbar(mappable, cax=cax, ticks=ticks)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
