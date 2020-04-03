---
title: matplotlib example find antipode v2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'find antipode v2'


Modules used in program: 
* `import shapely.validation`
* `import shapely.ops`
* `import shapely`
* `import matplotlib.pyplot`
* `import matplotlib.lines`
* `import matplotlib`
* `import cartopy`

## python find antipode v2

Python matplotlib example: find antipode v2

```python
#!/usr/bin/env python2
# -*- coding: utf-8 -*-

# Import modules ...
# NOTE: http://matplotlib.org/faq/howto_faq.html#matplotlib-in-a-web-application-server
import cartopy
import matplotlib
matplotlib.use("Agg")
import matplotlib.lines
import matplotlib.pyplot
import shapely
import shapely.ops
import shapely.validation

# Import my module ...
try:
    import pyguymer
except:
    raise Exception("you need to have the Python module from https://github.com/Guymer/PyGuymer located somewhere in your $PYTHONPATH")

# List countries to find the antipodes of and set run mode ...
avoids = [
    "United Kingdom",
    "United States",
]
debug = False

# Create colours and define number of angles and resolution ...
colours = ["red", "green", "blue", "cyan", "magenta", "yellow", "red", "green", "blue", "cyan", "magenta", "yellow"]
if debug:
    nang = 19
    simp = 0.1
else:
    nang = 181
    simp = 0.01

# Find file containing all the country shapes ...
if debug:
    shape_file = cartopy.io.shapereader.natural_earth(
        resolution = "110m",
        category = "cultural",
        name = "admin_0_countries"
    )
else:
    shape_file = cartopy.io.shapereader.natural_earth(
        resolution = "10m",
        category = "cultural",
        name = "admin_0_countries"
    )

# ******************************************************************************
# *                 CREATE MULTIPOLYGON TO FIND THE BUFFER OF                  *
# ******************************************************************************

# Create empty list ...
geoms = []

# Loop over records ...
for record in cartopy.io.shapereader.Reader(shape_file).records():
    # Check if this record is to be avoided ...
    # HACK: Depending on the resolution of the shape file that is loaded the
    #       country name is either "name" or "NAME".
    skip = True
    if "name" in record.attributes:
        if record.attributes["name"] in avoids:
            skip = False
    elif "NAME" in record.attributes:
        if record.attributes["NAME"] in avoids:
            skip = False
    else:
        raise Exception("country record doesn't have a name")

    # Skip this record if it is not a country to be avoided ...
    if skip:
        continue

    # Loop over geometries and append them to the list ...
    for geom in record.geometry.geoms:
        geoms.append(geom)

# Convert list to MultiPolygon ...
geoms = shapely.geometry.multipolygon.MultiPolygon(geoms)
if not geoms.is_valid:
    raise Exception(u"\"geoms\" is not valid")

# ******************************************************************************

# Create plot and make it pretty ...
fig = matplotlib.pyplot.figure(
    figsize = (9, 6),
        dpi = 300
)
ax = matplotlib.pyplot.axes(projection = cartopy.crs.Robinson())
ax.gridlines()
ax.set_global()
for record in cartopy.io.shapereader.Reader(shape_file).records():
    ax.add_geometries(
        record.geometry.geoms,
        cartopy.crs.PlateCarree(),
        edgecolor = "black",
        facecolor = (0, 0, 0, 0),
        linewidth = 0.5
    )
if debug:
    ax.coastlines(
        resolution = "110m",
        color = "black",
        linewidth = 0.5
    )
    pyguymer.add_map_background(ax)
else:
    ax.coastlines(
        resolution = "10m",
        color = "black",
        linewidth = 0.5
    )
    pyguymer.add_map_background(ax, resolution = "large4096px")

# Initialize distance, labels and lines ...
dist2 = 0.0                                                                     # [km]
labels = []
lines = []

# Loop over distances ...
for i in xrange(10):
    # Set distance ...
    if i >= 6:
        dist1 = 0.5e6                                                           # [m]
    else:
        dist1 = 2.0e6                                                           # [m]
    dist2 += 0.001 * dist1                                                      # [km]

    print("Calculating for {0:6,.0f} km ...".format(dist2))

    # Buffer area ...
    # NOTE: geoms is the [Multi]Polygon to be buffered.
    buffs = pyguymer.buffer_multipolygon(geoms, dist1, nang, simp, debug)
    # NOTE: buffs is now the [Multi]Polygon buffer of geoms.

    # Simplify shape ...
    geoms = pyguymer.simplify_poly(buffs, simp)
    # NOTE: geoms is now the simplified [Multi]Polygon buffer of geoms.

    # Add to plot ...
    ax.add_geometries(
        geoms,
        cartopy.crs.PlateCarree(),
        edgecolor = colours[i],
        facecolor = (0, 0, 0, 0),
        linewidth = 1.0
    )
    labels.append("{0:6,.0f} km".format(dist2))
    lines.append(matplotlib.lines.Line2D([], [], color = colours[i]))

print("Plotting ...")

# Save map as PNG ...
matplotlib.pyplot.legend(
    lines,
    labels,
    fontsize = "small",
    loc = "upper center",
    ncol = 5
)
matplotlib.pyplot.savefig(
    "find_antipode_v2.png",
    bbox_inches = "tight",
    dpi = 300,
    pad_inches = 0.1
)
matplotlib.pyplot.close("all")
if not debug:
    pyguymer.optipng("find_antipode_v2.png")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
