---
title: matplotlib example find airfields (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'find airfields'


Modules used in program: 
* `import shapely`
* `import matplotlib.pyplot`
* `import matplotlib.lines`
* `import matplotlib`
* `import cartopy`

## python find airfields

Python matplotlib example: find airfields

```python
#!/usr/bin/env python2
# -*- coding: utf-8 -*-

# Load modules ...
# NOTE: http://matplotlib.org/faq/howto_faq.html#matplotlib-in-a-web-application-server
import cartopy
import matplotlib
matplotlib.use("Agg")
import matplotlib.lines
import matplotlib.pyplot
import shapely

# Import my modules ...
try:
    import fmc
except:
    raise Exception("you need to have the Python module from https://github.com/Guymer/fmc located somewhere in your $PYTHONPATH")
try:
    import pyguymer
except:
    raise Exception("you need to have the Python module from https://github.com/Guymer/PyGuymer located somewhere in your $PYTHONPATH")

# Set run modes ...
debug = False

# Create short-hand for the colour map and define the nautical mile, number of angles and resolution ...
cmap = matplotlib.pyplot.get_cmap("jet")
nm = 1852.0                                                                     # [m/NM]
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

# Create a list of tuples of territories ...
# NOTE: The first entry is the territory name and the second is the list of
#       Natural Earth country names that make up the territory. The BAT has been
#       ignored as it has restrictions on its use.
territories = [
    ("United Kingdom"                            , ["United Kingdom"]           ),
    ("Akrotiri & Dhekelia"                       , ["Akrotiri", "Dhekelia"]     ),
    ("Anguilla"                                  , ["Anguilla"]                 ),
    ("Bermuda"                                   , ["Bermuda"]                  ),
    ("Cayman Islands"                            , ["Cayman Is."]               ),
    ("Falkland Islands"                          , ["Falkland Is."]             ),
    ("Gibraltar"                                 , ["Gibraltar"]                ),
    ("South Georgia & the South Sandwich Islands", ["S. Geo. and S. Sandw. Is."]),
    ("British Indian Ocean Territory"            , ["Br. Indian Ocean Ter."]    ),
    ("Montserrat"                                , ["Montserrat"]               ),
    ("Pitcairn Islands"                          , ["Pitcairn Is."]             ),
    ("Saint Helena, Ascension & Tristan da Cunha", ["Saint Helena"]             ),
    ("Turks and Caicos Islands"                  , ["Turks and Caicos Is."]     ),
    ("British Virgin Islands"                    , ["British Virgin Is."]       ),
]

# Create list of British polygons ...
polys = []

# Loop over records ...
for record in cartopy.io.shapereader.Reader(shape_file).records():
    # Skip this record if it is not for a territory in the list ...
    # HACK: Depending on the resolution of the shape file that is loaded the
    #       country name is either "name" or "NAME".
    trigger = False
    for territory in territories:
        if "name" in record.attributes:
            if record.attributes["name"] in territory[1]:
                trigger = True
                break
        elif "NAME" in record.attributes:
            if record.attributes["NAME"] in territory[1]:
                trigger = True
                break
        else:
            raise Exception("country record doesn't have a name")
    if not trigger:
        continue

    # Loop over geometries and add them to the list ...
    for geom1 in record.geometry.geoms:
        if isinstance(geom1, shapely.geometry.multipolygon.MultiPolygon):
            for geom2 in geom1.geoms:
                polys.append(pyguymer.buffer_polygon(geom2, 5000.0, nang, simp))# HACK: The Natural Earth polygons are sometimes so coarse that they do not cover the runway (e.g. Diego Garcia) so they are buffered by 5km.
        elif isinstance(geom1, shapely.geometry.polygon.Polygon):
            polys.append(pyguymer.buffer_polygon(geom1, 5000.0, nang, simp))    # HACK: The Natural Earth polygons are sometimes so coarse that they do not cover the runway (e.g. Diego Garcia) so they are buffered by 5km.
        else:
            raise Exception("\"geom1\" is an unexpected type")

print("There are {0:d} British polygons.".format(len(polys)))

# Create list of British airfields ...
# HACK: Manually add HLE as it is missing from the OpenFlights GitHub repository.
airfields = []                                                                  # [deg], [deg]
airfields.append((-5.645833, -15.959167))                                       # [deg], [deg]

# Loop over airports ...
for airport in fmc.load_airport_list():
    # Create point and check if it is within any of the polygons ...
    point = shapely.geometry.Point(airport["Longitude"], airport["Latitude"])   # [deg], [deg]
    for poly in polys:
        if poly.contains(point):
            airfields.append((point.x, point.y))                                # [deg], [deg]
            break

print("There are {0:d} British airfields.".format(len(airfields)))

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
dist2 = 0.0                                                                     # [NM]
labels = []
lines = []

# Loop over distances ...
for i in xrange(8):
    # Set distance ...
    dist1 = 500.0                                                               # [NM]
    dist2 += dist1                                                              # [NM]

    print("Calculating for {0:6,.0f} NM ...".format(dist2))

    # Decide what needs doing ...
    if i == 0:
        # Buffer airfields ...
        # NOTE: "airfields" is the Point-list to be buffered.
        buffs = pyguymer.buffer_points(airfields, dist1 * nm, nang, simp, debug)
        # NOTE: "buffs" is now the [Multi]Polygon buffer of "airfields".
    else:
        # Buffer area ...
        # NOTE: "geoms" is the [Multi]Polygon to be buffered.
        buffs = pyguymer.buffer_multipolygon(geoms, dist1 * nm, nang, simp, debug)
        # NOTE: "buffs" is now the [Multi]Polygon buffer of "geoms".

    # Simplify shape ...
    geoms = pyguymer.simplify_poly(buffs, simp)
    # NOTE: "geoms" is now the simplified [Multi]Polygon buffer of "airfields"/"geoms".

    # Add to plot ...
    ax.add_geometries(
        geoms,
        cartopy.crs.PlateCarree(),
        edgecolor = cmap(float(i) / 7.0),
        facecolor = (0, 0, 0, 0),
        linewidth = 1.0
    )
    labels.append("{0:6,.0f} NM".format(dist2))
    lines.append(matplotlib.lines.Line2D([], [], color = cmap(float(i) / 7.0)))

print("Plotting ...")

# Save map as PNG ...
matplotlib.pyplot.legend(
    lines,
    labels,
    fontsize = "small",
    loc = "upper center",
    ncol = 4
)
matplotlib.pyplot.savefig(
    "find_airfields.png",
    bbox_inches = "tight",
    dpi = 300,
    pad_inches = 0.1
)
matplotlib.pyplot.close("all")
if not debug:
    pyguymer.optipng("find_airfields.png")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
