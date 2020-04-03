---
title: matplotlib example mapplot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mapplot'

Functions in program: 
* `def map_countries(data, title=None, cmap=cm.jet, colorbar=True, vmin=None, vmax=None,`

Modules used in program: 
* `import sys`
* `import dbflib`
* `import matplotlib.pyplot as plt`
* `import matplotlib as mpl`
* `import numpy as np`

## python mapplot

Python matplotlib example: mapplot

```python
#!/usr/bin/python
# coding: utf-8
"""
Adapted from BaseMap example by geophysique.be
tutorial 07
http://www.geophysique.be/2011/01/27/matplotlib-basemap-tutorial-07-shapefiles-unleached/
"""

import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
from matplotlib.collections import LineCollection
from matplotlib import cm
from mpl_toolkits.basemap import Basemap
from shapelib import ShapeFile
import dbflib

import sys
PY3 = (sys.version_info[0] >= 3)

### PARAMETERS FOR MATPLOTLIB :
mpl.rcParams['font.size'] = 10.
mpl.rcParams['axes.labelsize'] = 8.
mpl.rcParams['xtick.labelsize'] = 6.
mpl.rcParams['ytick.labelsize'] = 6.

shp = ShapeFile("/home/thomas/tdwg3_borders/level3")
dbf = dbflib.open("/home/thomas/tdwg3_borders/level3")

def map_countries(data, title=None, cmap=cm.jet, colorbar=True, vmin=None, vmax=None,
                  nodata='grey', ice='white'):
    """
    Plot a world map of TDWG level 3 regions, coloured according to the data
    provided. Regions not included in the data will be coloured grey.
    
    data : mapping
      Mapping of TDWG level 3 codes to numeric values.
    title : str, optional
      A title for the plot.
    cmap : Matplotlib colour map, optional
    colorbar : bool
      If True, a colour bar will be drawn at the right.
    vmin, vmax : Set limits on the range of values. If not passed, they will be
      found from the data.
    """
    if vmin is None:
        vmin = min(data.values())
    if vmax is None:
        vmax = max(data.values())
    normalize = mpl.colors.Normalize(vmin=vmin, vmax=vmax)
    
    fig = plt.figure()
    #Custom adjust of the subplots
    plt.subplots_adjust(left=0.03,right=0.93,top=0.90,bottom=0.05,wspace=0.15,hspace=0.05)
    ax = plt.subplot(111)

    # Miller projection provides a visually pleasant map.
    m = Basemap(projection='mill',llcrnrlat=-80,urcrnrlat=85,\
                llcrnrlon=-180,urcrnrlon=180,lat_ts=20,resolution='c')
    #m.drawcountries(linewidth=0.5)
    #m.drawcoastlines(linewidth=0)
    m.drawparallels((0,),color='black',dashes=[1,1],linewidth=0.2) # draw parallels
    #m.drawmeridians(np.arange(x1,x2,2.), labels=[0,0,0,1], color='black',
    #        dashes=[1,0], labelstyle='+/-', linewidth=0.2) # draw meridians

    for npoly in range(shp.info()[0]):
        shpsegs = []

        shp_object = shp.read_object(npoly)
        verts = shp_object.vertices()
        shapedict = dbf.read_record(npoly)
        if PY3:
            name = shapedict['LEVEL3_NAM']
        else:
            name = shapedict["LEVEL3_NAM"].decode("cp1252")
        code = shapedict["LEVEL3_COD"]
        for ring in verts:
            lonlats = np.array(ring)
            lons, lats = lonlats[:,0], lonlats[:,1]
            if lons.max() > 721. or lons.min() < -721. or lats.max() > 91. or lats.min() < -91:
                raise ValueError
            shpsegs.append(np.vstack(m(lons, lats)).T)
        
        #print(name)
        lines = LineCollection(shpsegs,antialiaseds=(1,))
        if code in ('GNL', 'ANT'):
            # Icy regions - making these areas grey is distracting
            col = ice
        elif code in data:
            col = cmap(normalize(data[code]))
        elif code in ('NUN', 'SVA'):
            # Secondary icy regions - white if we don't have data for them.
            col = ice
        else:
            col = nodata
        
        lines.set_facecolors(col)
        lines.set_edgecolors('k')
        lines.set_linewidth(0.3)
        ax.add_collection(lines)
    
    if title:
        ax.set_title(title)
    
    # Kudos to Gavin Chait, in the comments of the above tutorial, for this bit
    if colorbar:
        bb = ax.get_position()
        ymid = (bb.y1+bb.y0)/2
        height = 0.6
        # [left, bottom, width, height]
        ax2 = fig.add_axes([bb.x1+0.01, ymid-(height/2), 0.014, height])

        cb = mpl.colorbar.ColorbarBase(ax2, cmap=cmap, norm=normalize, orientation='vertical')
        # Set the font size for the axis labels
        for t in cb.ax.get_yticklabels():
            t.set_fontsize(8)
        bbox_props = dict(boxstyle='round', fc='w', ec='0.5', alpha=0.8)
    
    return fig

if __name__ == "__main__":
    import argparse

    parser = argparse.ArgumentParser(description="Plot values for countries.")
    parser.add_argument('-i', '--integer', action='store_true',
                    help="Data values are integers, not floats")
    parser.add_argument('--min', type=float)
    parser.add_argument('--max', type=float)
    parser.add_argument('input', type=open, help='Data file (.csv)')
    parser.add_argument('output', nargs='?',
                    help='Filename to save image')
    
    args = parser.parse_args()
    
    import csv
    csvin = csv.reader(args.input)
    next(csvin)  # Discard header row
    if args.integer:
        data = {r[0]: int(r[1]) for r in csvin}
    else:
        data = {r[0]: float(r[1]) for r in csvin}
    fig = map_countries(data, vmin=args.min, vmax=args.max)
    
    if args.output:
        fig.savefig(args.output, bbox_inches='tight')
    else:
        fig.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
