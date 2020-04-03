---
title: matplotlib example weighted overlay (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'weighted overlay'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.image as mpimg`
* `import matplotlib.pyplot as plt`
* `import shapely`

## python weighted overlay

Python matplotlib example: weighted overlay

```python
# A script to convert vector data into a weighted raster overlay suitability map for prioritizing Libyan refugee camps locations.

# Python Library for importing geoJson
import shapely

# GeoPySpark imports
from geopyspark.geopycontext.GeoPyContext import GeoPyContext
from geopyspark.geotrellis.constants import SPATIAL, NODATAINT, MAX, SQUARE
from geopyspark.geotrellis.rdd import TiledRasterRDD

# matplotlib for rendering rasters
import matplotlib.pyplot as plt
import matplotlib.image as mpimg
import numpy as np

if __name__ == "__main__":
    # Create the context that we use to distribute the calculations
    geopysc = GeoPyContext(appName="libya", master="local[*]")

    # Loading vector datasets from GeoJSON files
    road_lines = shapely.open("road.geojson")
    libya_polygons = shapely.open("libya.geojson")
    conflict_points = shapely.open("conflict.geojson")
    population_points = shapely.open("population.geojson")

    # extent for our analysis area
    extent = {'xmin': 0.0, 'ymin': 0.0, 'xmax': 512.0, 'ymax': 512.0}

    # creating friction layer from GeoJson of roads
    road_raster_rdd = TiledRasterRDD.rasterize(geopysc, extent, "EPSG:3857", cols=256, rows=256, fill_value=1)
    # buffer the roads with a focal max to give them some width
    buffered_roads_raster_rdd = road_raster_rdd.focal(MAX, SQUARE, 1)
    # make roads lower friction than empty space
    road_friction_rdd = buffered_road_raster_rdd.reclassify(value_map={NODATAINT: 10, 1: 1}, value_type=int)

    # cost distance for conflicts / population centers along road network
    conflict_cost_distance_rdd = road_friction_rdd.cost_distance(geometries=conflict_points, max_distance=50000)
    population_cost_distance_rdd = road_friction_rdd.cost_distance(geometries=population_points, max_distance=50000)

    # Perform map algebra operations on tiled rasters to get weighted average
    mean_cost_distance_rdd = (conflict_cost_distance_rdd * 0.6 + population_cost_distance_rdd * 0.4)

    # Convert tiled raster rdd into a numpy array that can be rendered
    numpy_raster = mean_cost_distance_rdd.crop(extent)
    imgplot = plt.imshow(numpy_raster)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
