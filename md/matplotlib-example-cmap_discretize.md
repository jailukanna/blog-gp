---
title: matplotlib example cmap discretize (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'cmap discretize'

Functions in program: 
* `def cmap_discretize(cmap, N=10):`

Modules used in program: 
* `import matplotlib as mpl`
* `import numpy as np`

## python cmap discretize

Python matplotlib example: cmap discretize

```python
import numpy as np
import matplotlib as mpl
from matplotlib import cm

def cmap_discretize(cmap, N=10):

     cmap = cm.get_cmap(eval(cmap))
     colors_i = np.concatenate((np.linspace(0, 1., N), (0.,0.,0.,0.)))
     colors_rgba = cmap(colors_i)
     indices = np.linspace(0, 1., N+1)
     cdict = {}
     for ki,key in enumerate(('red','green','blue')):
        cdict[key] = [(indices[i], colors_rgba[i-1,ki], colors_rgba[i,ki]) for i in xrange(N+1)]

     return mpl.colors.LinearSegmentedColormap(cmap.name + "_%d"%N, cdict, 1024)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
