---
title: matplotlib example find antipode (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'find antipode'

Functions in program: 
* `def flip_geom(geom):`

Modules used in program: 
* `import shapely`
* `import matplotlib.pyplot`
* `import matplotlib`
* `import cartopy`

## python find antipode

Python matplotlib example: find antipode

```python
#!/usr/bin/env python2
# -*- coding: utf-8 -*-

# Import modules ...
# NOTE: http://matplotlib.org/faq/howto_faq.html#matplotlib-in-a-web-application-server
import cartopy
import matplotlib
matplotlib.use("Agg")
import matplotlib.pyplot
import shapely

# Import my module ...
try:
    import pyguymer
except:
    raise Exception("you need to have the Python module from https://github.com/Guymer/PyGuymer located somewhere in your $PYTHONPATH")

# List countries to find the antipodes of ...
avoids = [
    "United Kingdom",
    "United States",
]

# List global quadrants ...
quads = [
    shapely.geometry.polygon.Polygon(
        [
            (   0.0,   0.0),
            (   0.0,  90.0),
            ( 180.0,  90.0),
            ( 180.0,   0.0),
            (   0.0,   0.0),
        ]
    ),
    shapely.geometry.polygon.Polygon(
        [
            (   0.0,   0.0),
            (   0.0,  90.0),
            (-180.0,  90.0),
            (-180.0,   0.0),
            (   0.0,   0.0),
        ]
    ),
    shapely.geometry.polygon.Polygon(
        [
            (   0.0,   0.0),
            (   0.0, -90.0),
            ( 180.0, -90.0),
            ( 180.0,   0.0),
            (   0.0,   0.0),
        ]
    ),
    shapely.geometry.polygon.Polygon(
        [
            (   0.0,   0.0),
            (   0.0, -90.0),
            (-180.0, -90.0),
            (-180.0,   0.0),
            (   0.0,   0.0),
        ]
    ),
]

# Define function ...
def flip_geom(geom):
    # Find limits of geometry ...
    xmin, ymin, xmax, ymax = geom.bounds                                        # [deg], [deg], [deg], [deg]

    # Create list to hold its transposed coordinates ...
    coords = []

    # Loop over coordinates in the external ring, transpose them and add them to the list ...
    for coord in geom.exterior.coords:
        x, y = coord                                                            # [deg], [deg]
        if xmin <= 0.0 and xmax <= 0.0:
            coords.append((x + 180.0, -y))
        elif xmin >= 0.0 and xmax >= 0.0:
            coords.append((x - 180.0, -y))
        else:
            raise Exception("the geom crosses the anti-meridian")

    # Check that coordinates were added ...
    if len(coords) == 0:
        raise Exception("no coords were added")

    # Return polygon from the transposed coordinates of the geometry ...
    return shapely.geometry.polygon.Polygon(coords)

# Create plot and make it pretty ...
fig = matplotlib.pyplot.figure(
    figsize = (9, 6),
        dpi = 300
)
ax = matplotlib.pyplot.axes(projection = cartopy.crs.Robinson())
ax.set_global()
pyguymer.add_map_background(ax, resolution = "large4096px")
ax.coastlines(
    resolution = "10m",
    color = "black",
    linewidth = 0.1
)

# Add notable lines of latitude manually ...
y1 = 66.0 + 33.0 / 60.0 + 46.2 / 3600.0                                         # [deg]
y2 = 23.0 + 26.0 / 60.0 + 13.8 / 3600.0                                         # [deg]
matplotlib.pyplot.plot(
    [-180.0, 180.0],
    [ y1,  y1],
    transform = cartopy.crs.PlateCarree(),
    color = "black",
    linewidth = 0.1,
    linestyle = ":"
)
matplotlib.pyplot.plot(
    [-180.0, 180.0],
    [ y2,  y2],
    transform = cartopy.crs.PlateCarree(),
    color = "black",
    linewidth = 0.1,
    linestyle = ":"
)
matplotlib.pyplot.plot(
    [-180.0, 180.0],
    [0.0, 0.0],
    transform = cartopy.crs.PlateCarree(),
    color = "black",
    linewidth = 0.1,
    linestyle = ":"
)
matplotlib.pyplot.plot(
    [-180.0, 180.0],
    [-y2, -y2],
    transform = cartopy.crs.PlateCarree(),
    color = "black",
    linewidth = 0.1,
    linestyle = ":"
)
matplotlib.pyplot.plot(
    [-180.0, 180.0],
    [-y1, -y1],
    transform = cartopy.crs.PlateCarree(),
    color = "black",
    linewidth = 0.1,
    linestyle = ":"
)

# Find file containing all the country shapes ...
shape_file = cartopy.io.shapereader.natural_earth(
    resolution = "10m",
    category = "cultural",
    name = "admin_0_countries"
)

# Loop over records ...
for record in cartopy.io.shapereader.Reader(shape_file).records():
    # Set country opacity ...
    # HACK: Depending on the resolution of the shape file that is loaded the
    #       country name is either "name" or "NAME".
    alpha = 0.25
    if "name" in record.attributes:
        if record.attributes["name"] in avoids:
            alpha = 0.5
    elif "NAME" in record.attributes:
        if record.attributes["NAME"] in avoids:
            alpha = 0.5
    else:
        raise Exception("country record doesn't have a name")

    # Create list to hold the transposed polygons ...
    polys = []

    # Loop over geometries ...
    for geom in record.geometry.geoms:
        # Find bounds ...
        xmin, ymin, xmax, ymax = geom.bounds                                    # [deg], [deg], [deg], [deg]

        # Check if this geometry needs splitting ...
        # NOTE: See https://en.wikipedia.org/wiki/180th_meridian#Software_representation_problems
        if not (xmin < 0.0 and xmax < 0.0) and not (xmin > 0.0 and xmax > 0.0):
            # Loop over quadrants ...
            for quad in quads:
                # Find intersection of geometry with quadrant ...
                tmp1 = geom.intersection(quad)

                # Skip this intersection if it is empty ...
                if tmp1.is_empty:
                    continue

                # Check how many polygons describe the intersection ...
                if isinstance(tmp1, shapely.geometry.multipolygon.MultiPolygon):
                    # Loop over sub-intersections ...
                    for tmp2 in tmp1:
                        # Add flipped sub-intersection to list ...
                        polys.append(flip_geom(tmp2))
                else:
                    # Add flipped intersection to list ...
                    polys.append(flip_geom(tmp1))
        else:
            # Add flipped geometry to list ...
            polys.append(flip_geom(geom))

    # Fill the country in ...
    if len(polys) > 0:
        ax.add_geometries(
            polys,
            cartopy.crs.PlateCarree(),
            alpha = alpha,
            edgecolor = "red",
            facecolor = "red",
            linewidth = 0.5
        )

# Save map as PNG and optimise it ...
matplotlib.pyplot.savefig(
    "find_antipode.png",
    bbox_inches = "tight",
    dpi = 300,
    pad_inches = 0.1
)
matplotlib.pyplot.close("all")
pyguymer.optipng("find_antipode.png")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
