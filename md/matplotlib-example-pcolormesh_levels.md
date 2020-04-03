---
title: matplotlib example pcolormesh levels (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'pcolormesh levels'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python pcolormesh levels

Python matplotlib example: pcolormesh levels

```python
import matplotlib.pyplot as plt
from matplotlib.colors import BoundaryNorm
from matplotlib.ticker import MaxNLocator
import numpy as np
plt.rcParams['savefig.dpi']=100

# make these smaller to increase the resolution
dx, dy = 0.05, 0.05
y, x = np.mgrid[slice(1, 5 + dy, dy),
                slice(1, 5 + dx, dx)]
z = np.cos(x)**10 + np.sin(10 + y*x) * np.sin(x)
z = z[:-1, :-1]

levels = MaxNLocator(nbins=15).tick_values(z.min(), z.max())

# pick the desired colormap, sensible levels, and define a normalization
# instance which takes data values and translates those into levels.
cmap = plt.get_cmap('PuOr')
norm = BoundaryNorm(levels, ncolors=cmap.N, clip=True)

fig, (ax0, ax1) = plt.subplots(nrows=2,figsize=(5,7))

im = ax0.pcolormesh(x, y, z, cmap=cmap, norm=norm)
fig.colorbar(im, ax=ax0)
ax0.set_title('pcolormesh with levels')

# contours are *point* based plots, so convert our bound into point centers
cf = ax1.contourf(x[:-1, :-1] + dx/2.,
                  y[:-1, :-1] + dy/2., z, levels=levels,
                  cmap=cmap)
fig.colorbar(cf, ax=ax1)
ax1.set_title('contourf with levels')
fig.tight_layout()
#plt.savefig("pcolormesh_vs_contourf.png",bbox_inches = 'tight', pad_inches = 0)
plt.show()



#random z data 
import matplotlib.pyplot as plt
from matplotlib.colors import BoundaryNorm
from matplotlib.ticker import MaxNLocator
import numpy as np


# make these smaller to increase the resolution
dx, dy = 0.05, 0.05

y, x = np.mgrid[slice(1, 5 + dy, dy),
                slice(1, 5 + dx, dx)]
z = np.random.rand(x.shape[0],x.shape[1])
z = z[:-1, :-1]
levels = MaxNLocator(nbins=10).tick_values(z.min(), z.max())

cmap = plt.get_cmap('PuOr')
norm = BoundaryNorm(levels, ncolors=cmap.N, clip=True)

fig, (ax0, ax1) = plt.subplots(nrows=2,figsize=(5,7))

im = ax0.pcolormesh(x, y, z, cmap=cmap, norm=norm)
fig.colorbar(im, ax=ax0)
ax0.set_title('pcolormesh with levels')

cf = ax1.contourf(x[:-1, :-1] + dx/2.,
                  y[:-1, :-1] + dy/2., z, levels=levels,
                  cmap=cmap)
fig.colorbar(cf, ax=ax1)
ax1.set_title('contourf with levels')
fig.tight_layout()
#plt.savefig("pcolormesh_vs_contourf_ransu.png",bbox_inches = 'tight', pad_inches = 0)
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
