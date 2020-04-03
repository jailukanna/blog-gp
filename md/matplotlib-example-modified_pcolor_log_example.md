---
title: matplotlib example modified pcolor log example (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'modified pcolor log example'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python modified pcolor log example

Python matplotlib example: modified pcolor log example

```python
"""
Based on the pcolor_log example:
http://matplotlib.org/examples/pylab_examples/pcolor_log.html

More about colormaps here:
http://matplotlib.org/users/colormaps.html
"""

import numpy as np
import matplotlib.pyplot as plt
from matplotlib.colors import LogNorm
from matplotlib.mlab import bivariate_normal  # to create dummy data
from matplotlib.ticker import NullLocator  # for axes cosmeticks

# Dummy data
N = 100
X, Y = np.mgrid[-3:3:N*1j, -2:2:N*1j]
Z = (bivariate_normal(X, Y, 0.1, 0.2, 1.0, 1.0) +
     0.1 * bivariate_normal(X, Y, 1.0, 1.0, 0.0, 0.0))
# Make outliers of some random points
np.random.seed(123456)
outliers = np.random.randint(low=0, high=N, size=(25, 2))
Z[outliers[:, 0], outliers[:, 1]] *= 1000

fig, axs = plt.subplots(nrows=2, ncols=2, figsize=(9.6, 6.4))
for (ax, cmap) in zip(axs.flat, ('viridis', 'gnuplot2', 'cubehelix', 'brg')):
    # Display each matrix element with a color (in a log. scale)
    im = ax.pcolor(X, Y, Z, norm=LogNorm(vmin=Z.min(), vmax=Z.max()),
                   cmap=cmap)
    plt.colorbar(im, ax=ax)
    # Remove the axes ticks for my eyes pleasure
    ax.xaxis.set_major_locator(NullLocator())
    ax.yaxis.set_major_locator(NullLocator())

plt.tight_layout()
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
