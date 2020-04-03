---
title: matplotlib example sub plot pcolor (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sub plot pcolor'

Functions in program: 
* `def sub_plot_pcolor(lons, lats, data, title=None, cmap=cm.jet,`

Modules used in program: 
* `import numpy as np`
* `import mpl_toolkits.basemap.cm as GMT`
* `import matplotlib.pyplot as plt`
* `import matplotlib as mpl`

## python sub plot pcolor

Python matplotlib example: sub plot pcolor

```python
import matplotlib as mpl
import matplotlib.pyplot as plt
from matplotlib import cm
from mpl_toolkits.basemap import Basemap
import mpl_toolkits.basemap.cm as GMT
# import xray
import numpy as np

def sub_plot_pcolor(lons, lats, data, title=None, cmap=cm.jet,
                    vmin=None, vmax=None, cbar=True, cbar_location='bottom',
                    units=None):
    

    if vmin is None:
        vmin=data.min()
    if vmax is None:
        vmax=data.max()

    projection = {'urcrnrlat': 27.511827255753555, 
                  'urcrnrlon': 16.90845094934209, 
                  'llcrnrlat': 16.534986367884521, 
                  'llcrnrlon': 189.2229322311162, 
                  'projection': 'lcc', 
                  'rsphere': 6371200.0, 
                  'lon_0': -114, 
                  'lat_0': 90}
        
    m = Basemap(**projection)
    xi,yi = m(np.squeeze(lons),np.squeeze(lats))
    sp = m.pcolormesh(xi, yi, np.squeeze(data), vmin=vmin,vmax=vmax, cmap=cmap)
    m.drawparallels(np.arange(-80.,81.,20.))
    m.drawmeridians(np.arange(-180.,181.,20.))
    m.drawcoastlines(color='k',linewidth=0.25);
    
    if title:
        plt.title(title,size=13)
    if cbar:
        cbar = m.colorbar(location=cbar_location)
    cbar.set_label(units)
    
    return sp

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
