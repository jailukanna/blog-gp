---
title: matplotlib example betterstep (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'betterstep'

Functions in program: 
* `def betterstep(bins, y, **kwargs):`

Modules used in program: 
* `import matplotlib.pyplot as plt`

## python betterstep

Python matplotlib example: betterstep

```python
import matplotlib.pyplot as plt

def betterstep(bins, y, **kwargs):
    """A 'better' version of matplotlib's step function
    
    Given a set of bin edges and bin heights, this plots the thing
    that I wish matplotlib's ``step`` command plotted. All extra
    arguments are passed directly to matplotlib's ``plot`` command.
    
    Args:
        bins: The bin edges. This should be one element longer than
            the bin heights array ``y``.
        y: The bin heights.
        ax (Optional): The axis where this should be plotted.
    
    """
    new_x = [a for row in zip(bins[:-1], bins[1:]) for a in row]
    new_y = [a for row in zip(y, y) for a in row]
    ax = kwargs.pop("ax", plt.gca())
    return ax.plot(new_x, new_y, **kwargs)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
