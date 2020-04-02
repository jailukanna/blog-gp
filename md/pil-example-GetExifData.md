---
title: pil example GetExifData (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'GetExifData'

Functions in program: 
* `def get_coordinates(geotags):`
* `def get_decimal_from_dms(dms, ref):`
* `def get_geotagging(exif):`
* `def get_labeled_exif(exif):`
* `def get_exif(filename):`

Modules used in program: 
* `import pprint`

## python GetExifData

Python pil example: GetExifData

```python
from PIL import Image
from PIL.ExifTags import TAGS
from PIL.ExifTags import GPSTAGS
import pprint

#get exif data from images
def get_exif(filename):
    image = Image.open(filename)
    image.verify()
    return image._getexif()

#converted data into human readable with exif tags
def get_labeled_exif(exif):
    labeled = {}
    for (key, val) in exif.items():
        labeled[TAGS.get(key)] = val
    return labeled

#get only gps information 
def get_geotagging(exif):
    geotagging = {}
    for (idx, tag) in TAGS.items():
        if tag == 'GPSInfo':
            if idx not in exif:
                print("No EXIF geotagging found")
        for (key, val) in GPSTAGS.items():
            if key in exif[idx]:
                geotagging[val] = exif[idx][key]
    return geotagging

#get DMS value from gps Exif data  google search for 'DMS lat long'
def get_decimal_from_dms(dms, ref):

    degrees = dms[0][0] / dms[0][1]
    minutes = dms[1][0] / dms[1][1] / 60.0
    seconds = dms[2][0] / dms[2][1] / 3600.0

    if ref in ['S', 'W']:
        degrees = -degrees
        minutes = -minutes
        seconds = -seconds

    return round(degrees + minutes + seconds, 5)

def get_coordinates(geotags):
    lat = get_decimal_from_dms(geotags['GPSLatitude'], geotags['GPSLatitudeRef'])
    lon = get_decimal_from_dms(geotags['GPSLongitude'], geotags['GPSLongitudeRef'])

    return (lat,lon)

exif = get_exif("greenPatta.jpg")
data = get_labeled_exif(exif)
activeTags = ['DateTime', 'DateTimeDigitized', 'ExifImageHeight', 'ExifImageWidth', 'Make', 'Model', 'Software', 'GPSInfo']
dictWithActiveTags = { activeKey: data[activeKey] for activeKey in activeTags }
pprint.pprint(dictWithActiveTags)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
