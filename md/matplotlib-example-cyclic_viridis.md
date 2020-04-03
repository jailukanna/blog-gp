---
title: matplotlib example cyclic viridis (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'cyclic viridis'


Modules used in program: 
* `import matplotlib.colors`
* `import matplotlib.cm`

## python cyclic viridis

Python matplotlib example: cyclic viridis

```python
import matplotlib.cm
import matplotlib.colors

cyclic_viridis = matplotlib.colors.LinearSegmentedColormap.from_list(
    'cyclic_viridis',
    [(0, matplotlib.cm.viridis.colors[0]),
     (0.25, matplotlib.cm.viridis.colors[256 // 3]),
     (0.5, matplotlib.cm.viridis.colors[2 * 256 // 3]),
     (0.75, matplotlib.cm.viridis.colors[-1]),
     (1.0, matplotlib.cm.viridis.colors[0])])


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
