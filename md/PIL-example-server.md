---
title: PIL example server (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'server'

Functions in program: 
* `def test():`
* `def hello_world():`
* `def create_heatmap():`
* `def sp_law_of_cosines(lonLat1, lonLat2):`
* `def haversine(lonLat1, lonLat2):`
* `def serve_pil_image(pil_img):`

Modules used in program: 
* `import pyproj`
* `import numpy as np`

## python server

Python PIL example: server

```python
from flask import Flask
from flask import send_file
from StringIO import StringIO
import numpy as np
from PIL import Image
import pyproj
from pyproj import Proj
from math import radians, cos, sin, asin, sqrt, acos, degrees

def serve_pil_image(pil_img):
    img_io = StringIO()
    pil_img.save(img_io, 'PNG', quality=70)
    img_io.seek(0)
    return send_file(img_io, mimetype='image/png')

def haversine(lonLat1, lonLat2):
    lon1, lat1 = map(radians, lonLat1)
    lon2, lat2 = map(radians, lonLat2)
    dlon = lon2 - lon1
    dlat = lat2 - lat1
    a = sin(dlat/2)**2 + cos(lat1) * cos(lat2) * sin(dlon/2)**2
    c = 2 * asin(sqrt(a))
    earth_radius_miles = 3964
    return c * earth_radius_miles

def sp_law_of_cosines(lonLat1, lonLat2):
    lon1, lat1 = lonLat1
    lon2, lat2 = lonLat2
    try:
        delta = lon2 - lon1
        a = radians(lat1)
        b = radians(lat2)
        C = radians(delta)
        x = sin(a) * sin(b) + cos(a) * cos(b) * cos(C)
        distance = acos(x) # in radians
        distance  = degrees(distance) # in degrees
        distance  = distance * 60 # 60 nautical miles / lat degree
        return distance;
    except:
        return 0

def create_heatmap():
    p1 = Proj(proj='latlong',datum='WGS84')
    p2 = Proj(init='epsg:3857')
    # WebMercator cuts off at 85.051129 lat and 180 long.
    # The magic number was determined experimentally by
    # finding the constant that makes the lat long cutoffs
    # correspond to the image size cutoffs.
    MAGIC_NUMBER = 40000000
    height=256
    width=256
    data = np.zeros((height, width, 4), dtype=np.uint8)
    for x in range(width):
        for y in range(height):
            pixel_in_webmerc = (
                (-x + float(width)/2) * MAGIC_NUMBER / width,
                (-y + float(height)/2) * MAGIC_NUMBER / height)
            projected_point = pyproj.transform(p2, p1,
                                               pixel_in_webmerc[0],
                                               pixel_in_webmerc[1])
            distance = sp_law_of_cosines((122.3, 47.6), projected_point)
            data[y, x] += map(np.uint8, [0,255,0, 255 / (distance/100 + 1)])
    i = Image.fromarray(data, 'RGBA')
    return i

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World!'

@app.route('/test.png')
def test():
    return serve_pil_image(create_heatmap())

if __name__ == '__main__':
    app.run(host='0.0.0.0')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
