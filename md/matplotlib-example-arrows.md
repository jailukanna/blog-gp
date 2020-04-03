---
title: matplotlib example arrows (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'arrows'

Functions in program: 
* `def arrows(ax):`

## python arrows

Python matplotlib example: arrows

```python
from cartopy.examples.arrows import sample_data


def arrows(ax):
    x, y, u, v, vector_crs = sample_data()
    ax.quiver(x, y, u, v, transform=vector_crs)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
