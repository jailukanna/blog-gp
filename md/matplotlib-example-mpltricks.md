---
title: matplotlib example mpltricks (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mpltricks'

Functions in program: 
* `def truncate_colormap(cmap, minval=0.0, maxval=1.0, n=100):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib`

## python mpltricks

Python matplotlib example: mpltricks

```python
import matplotlib
import matplotlib.pyplot as plt

def truncate_colormap(cmap, minval=0.0, maxval=1.0, n=100):
    """Clip the top and/or bottom off a colormap.
    
    Args:
      cmap (matplotlib.colors.ListedColormap): E.g. plt.cm.viridis
      minval (number): [0 - 1]. 'Bottom' of the colormap.
      maxval (number): [0 - 1]. 'Top' of the colormap.
      n (int): Sets how smooth the colormap will be. Higher is smoother.

    Notes:
      Taken from https://stackoverflow.com/questions/18926031
    """
    return matplotlib.colors.LinearSegmentedColormap.from_list(
        'trunc({n},{a:.2f},{b:.2f})'.format(
          n=cmap.name, a=minval, b=maxval), cmap(np.linspace(minval, maxval, n)))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
