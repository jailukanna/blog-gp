---
title: matplotlib example lasso imshow (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'lasso imshow'

Functions in program: 
* `def onselect(verts):`

Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python lasso imshow

Python matplotlib example: lasso imshow

```python
#!/usr/bin/env python

"""
How to interactively select part of an array displayed as an image with matplotlib.
"""

import matplotlib.pyplot as plt
from matplotlib.path import Path
from matplotlib.widgets import LassoSelector
import numpy as np
from skimage.data import coins

data = coins()
fig = plt.figure()
ax1 = fig.add_subplot(121)
ax1.imshow(data)
ax2 = fig.add_subplot(122)
ax2.imshow(np.zeros_like(data))
plt.subplots_adjust()

x, y = np.meshgrid(np.arange(data.shape[1]), np.arange(data.shape[0]))
pix = np.vstack((x.flatten(), y.flatten())).T

def onselect(verts):

    # Select elements in original array bounded by selector path:
    p = Path(verts)
    ind = p.contains_points(pix, radius=1)
    selected = np.zeros_like(data)
    selected.flat[ind] = data.flat[ind]
    ax2.imshow(selected)
    fig.canvas.draw_idle()
    
lasso = LassoSelector(ax1, onselect)

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
