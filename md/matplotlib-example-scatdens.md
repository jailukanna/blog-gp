---
title: matplotlib example scatdens (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'scatdens'

Functions in program: 
* `def scatter_density_plot(xx, yy, xrange, yrange, mode="hist", sort=True, modepars={}, ax=None, fig=plt, **kwargs):`

Modules used in program: 
* `import matplotlib as mpl`
* `import matplotlib.path as mpath`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import seaborn as sb`

## python scatdens

Python matplotlib example: scatdens

```python
from scipy.interpolate import interpn
from scipy.stats import gaussian_kde
from scipy.spatial import cKDTree

import seaborn as sb
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.path as mpath
from matplotlib.colors import Normalize
from matplotlib import cm
import matplotlib as mpl

def scatter_density_plot(xx, yy, xrange, yrange, mode="hist", sort=True, modepars={}, ax=None, fig=plt, **kwargs):
    grid = np.vstack([xx,yy])
    ax = plt.gca() if ax is None else ax
    
    plotargs = {}
    
    if mode == "hist":
        bins = modepars.get('bins', (15,15))
        h, x_e, y_e = np.histogram2d(xx, yy, bins=bins)
        
        stepx = x_e[1]-x_e[0]
        x_e = np.pad(x_e, (1,1), 'constant', constant_values=(x_e[0]-stepx, x_e[-1]+stepx))
        stepy = y_e[1]-y_e[0]
        y_e = np.pad(y_e, (1,1), 'constant', constant_values=(y_e[0]-stepx, y_e[-1]+stepx))
        h = np.pad(h, ((1,1), (1,1)), 'constant', constant_values=((0,0), (0,0)))
        xc = (x_e[1:] + x_e[:-1])/2
        yc = (y_e[1:] + y_e[:-1])/2
        
        zz = interpn((xc, yc), h, grid.T, method = "linear")
        
    elif mode == "kde":
        kernel = gaussian_kde(grid) 
        zz = kernel(grid)
        
        
    elif mode == "near":
        scalingx = (xrange[1] - xrange[0])
        scalingy = (yrange[1] - yrange[0])
        xxn = (xx - xrange[0]) / scalingx
        yyn = (yy - yrange[0]) / scalingy
        
        gridn = np.vstack([xxn, yyn])
        tree = cKDTree(gridn.T)
        searchradius = modepars.get('searchradius', 0.03)
        njobs = modepars.get('njobs', 2)
    
        nb = tree.query_ball_point(gridn.T, searchradius, n_jobs=njobs)
        zz = np.fromiter(map(len, nb), dtype=int)
        
        circle = mpath.Path.unit_circle()
        verts = np.copy(circle.vertices)
        xp1, yp1, xp2, yp2 = ax.figure.bbox.bounds
        ap = (xp2 - xp1) * searchradius
        bp = (yp2 - yp1) * searchradius
        
        verts[:, 0] *= ap/bp
        verts[:, 1] *= 1
        
        ell_marker = mpath.Path(verts, circle.codes)
        
        area_p = ap*bp*np.pi
        plotargs['s'] = kwargs.pop('s', area_p)
        plotargs['marker'] = kwargs.pop('marker', ell_marker)
    else:
        raise ValueError(f"Invalid mode: {mode}")
    
    if plotargs.get('s', None) == 'auto':
        plotargs['s'] = mpl.rcParams['lines.markersize'] ** 2
    
    if sort:
        idx = np.argsort(zz)
        xx = np.array(xx)[idx]
        yy = np.array(yy)[idx]
        zz = zz[idx]
        
    mapping = ax.scatter(xx, yy, c=zz, **plotargs, **kwargs)
    ax.set_xlim(*xrange)
    ax.set_ylim(*yrange)
    fig.colorbar(mapping, ax=ax, extend='max')
    return ax


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
