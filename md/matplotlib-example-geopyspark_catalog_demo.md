---
title: matplotlib example geopyspark catalog demo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'geopyspark catalog demo'

Functions in program: 
* `def show_layer(rdd, width = 18, height = 11):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib.pyplot as plt`
* `import json`

## python geopyspark catalog demo

Python matplotlib example: geopyspark catalog demo

```python
from geopyspark.geopycontext import GeoPyContext
from geopyspark.geotrellis.constants import SPATIAL, NODATAINT
from geopyspark.geotrellis.rdd import TiledRasterRDD

geopysc = GeoPyContext(appName="libya", master="local[*]")

import json


from geopyspark.geotrellis.geotiff_rdd import get
rdd = get(geopysc, SPATIAL, 's3://geopyspark-test/example-files/cropped.tif')

import matplotlib.pyplot as plt
%matplotlib inline

recs = rdd.to_numpy_rdd().collect()
tile = recs[0][1]['data']
plt.imshow(tile[0], cmap="hot", aspect='equal')

from geopyspark.geotrellis import catalog
income = catalog.read(geopysc, SPATIAL, "s3://azavea-datahub/catalog", "us-census-median-household-income-30m-epsg3857", 5)



from geopyspark.geotrellis.geotiff_rdd import get
rdd = get(geopysc, SPATIAL, 's3://geopyspark-test/example-files/cropped.tif')

import matplotlib.pyplot as plt
%matplotlib inline

recs = rdd.to_numpy_rdd().collect()
tile = recs[0][1]['data']
plt.imshow(tile[0], cmap="hot", aspect='equal')



# Load layers from already ingested GeoTrellis catalog

from geopyspark.geotrellis import catalog

household_income = catalog.read(geopysc, SPATIAL, "s3://azavea-datahub/catalog",
                      "us-census-median-household-income-30m-epsg3857", 4)

property_value = catalog.read(geopysc, SPATIAL, 's3://azavea-datahub/catalog',
                      'us-census-property-value-30m-epsg3857', 4)



def show_layer(rdd, width = 18, height = 11):
    tile = rdd.stitch()
    numpy_array = tile['data'][0]
    fig=plt.figure(figsize=(width, height), dpi= 80, facecolor='w', edgecolor='k')
    return plt.imshow(numpy_array, aspect='auto')


show_layer(household_income)



# Query for tiles intersecting polygon from catalog
from shapely.geometry import Polygon
from shapely.geometry import asShape

pa_extent = """{"type":"Polygon","coordinates":[[[-8964534.67728547,4806360.338571883],[-8304118.752901547,4806360.338571883],[-8304118.752901547,5183042.0139612295],[-8964534.67728547,5183042.0139612295],[-8964534.67728547,4806360.338571883]]]}"""
query_bbox = asShape(json.loads(pa_extent))
pa_income = catalog.query(geopysc, SPATIAL, "s3://azavea-datahub/catalog",
                      "us-census-median-household-income-30m-epsg3857", 5, query_bbox)
show_layer(pa_income,12,10)


show_layer(property_value)


show_layer(household_income)


wo = (household_income * 0.2 + property_value * 0.8)
show_layer(wo)


from geopyspark.geotrellis.constants import SUM, SQUARE

show_layer(wo.focal(SUM, SQUARE, 2.0))


from shapely.geometry import Point, asShape

bounds_geojson = """
{"type":"Polygon","coordinates":[[[1310824.5612687299,1947824.2414807738],[1806182.8452710845,2045148.2265168347],[1742209.1902140433,2324386.1702995948],[1264396.1287639816,2230509.328537968],[1310824.5612687299,1947824.2414807738]]]}"""

us_slope = catalog.query(geopysc, SPATIAL, "s3://azavea-datahub/catalog",
                      "us-percent-slope-30m-epsg5070", 0, asShape(json.loads(bounds_geojson)))

cd = us_slope.cost_distance(geometries=[Point(1310824.5612518974, 1947824.2415832607)], max_distance=float('inf'))

show_layer(cd)


from shapely.geometry import Point, asShape
from geopyspark.geotrellis import catalog

bounds_geojson = """
{"type":"Polygon","coordinates":[[[1690687.494224459,2101022.877118857],[1702223.730913837,2103523.395067412],[1699955.8286372246,2113949.329694799],[1688434.961876591,2111452.1432297896],[1690687.494224459,2101022.877118857]]]}
"""

us_slope =
catalog.query(geopysc, SPATIAL,
              "s3://azavea-datahub/catalog",
              "us-percent-slope-30m-epsg5070", 0,
              asShape(json.loads(bounds_geojson)))

cd = us_slope.cost_distance(geometries=[Point(-75.68069458007812,40.37688995771414)], max_distance=float('inf'))

#show_layer(us_slope)
#show_layer(cd)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
