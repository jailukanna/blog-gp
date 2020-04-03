---
title: matplotlib example wktplotpoly (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'wktplotpoly'

Functions in program: 
* `def main():`
* `def getPolygons(fn, key):`

Modules used in program: 
* `import re `
* `import numpy as np`
* `import csv`
* `import matplotlib.path as mpath`
* `import matplotlib.patches as mpatches`
* `import matplotlib.pyplot as plt`

## python wktplotpoly

Python matplotlib example: wktplotpoly

```python
#!/usr/bin/python

"""
Uses matplotlib to plot WKT polygons in CSV format.

1. install GDAL
2. download a shapefile
3. use ogr2ogr to convert it to CSV with WKT
   ogr2ogr -f CSV output.csv input.shp -lco GEOMETRY=AS_WKT
4. run getPolygons against the output csv

"""


import matplotlib.pyplot as plt
import matplotlib.patches as mpatches
import matplotlib.path as mpath


from matplotlib.collections import PatchCollection

import csv
import numpy as np
import re 



def getPolygons(fn, key):
    polys = []
    with open(fn) as f:
        reader = csv.DictReader(f)
        polys = []
        areas = {}
        for row in reader:
            print(row.keys())
            arean = int(row[key]) #
            wkt = row['WKT']
            # ignore interior rings
            if wkt.startswith("POLYGON"):
                wktm = re.match("POLYGON \(\(((?:[0-9.,]|\s)+)\)", wkt)
                if wktm:
                    wktt = wktm.group(1)
                    wktt = [x.split() for x in wktt.split(",")]
                    points = np.array([map(float, x) for x in wktt])
                    points = points.astype(int) # ints seem okay so far
                    areas[arean]  = mpatches.Polygon(points)
            elif wkt.startswith("MULTIPOLYGON"):
                # OHARE dammit
                subparts = re.findall("\(\(((?:[0-9.,]|\s)+)\)", wkt)
                points, codes = [], []
                for sub in subparts:
                    wktt = [x.split() for x in sub.split(",")]
                    # first elt of each polygon's code is a MOVETO, rest are LINETO
                    cs = [mpath.Path.LINETO] * len(wktt)
                    cs[0] = mpath.Path.MOVETO
                    codes += cs
                    points += [map(float, x) for x in wktt]
                ppts = np.array(points).astype(int)
                    
                p = mpath.Path(ppts, np.array(codes))
                areas[arean] = mpatches.PathPatch(p)

    polys = areas.items()
    polys.sort()
    
    return PatchCollection([p for n,p in polys])
    

def main():
    import matplotlib.cm as cm
    import numpy.random as npr

    # this setup code is probably necessary for reasonable use
    fig = plt.figure(facecolor="white")
    ax= plt.axes()

    # pick random values for each CA
    values = npr.rand(700)

    coll = getPolygons("output3.csv", key='TRACTCE10')
        
    # color the polys using the random values
    coll.set_cmap(cm.Reds)
    coll.set_array(np.array(values))
    coll.set_alpha(0.6) # for pretty

    ax.add_collection(coll)
    plt.axis('equal')
    plt.axis('off')
    plt.show()

if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
