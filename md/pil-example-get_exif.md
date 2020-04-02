---
title: pil example get exif (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'get exif'

Functions in program: 
* `def get_exif(image_path):`
* `def _map_key(k):`

## python get exif

Python pil example: get exif

```python
from PIL import Image
from PIL.ExifTags import GPSTAGS
from PIL.ExifTags import TAGS

# Keys are listed here:
# https://github.com/python-pillow/Pillow/blob/master/PIL/ExifTags.py

def _map_key(k):
    try:
        return TAGS[k]
    except KeyError:
        return GPSTAGS.get(k, k)

def get_exif(image_path):
    metadata = {}
    with Image.open(image_path) as i:
        info = i._getexif()
    try:
        [ metadata.__setitem__(_map_key(k), v) for k, v in info.items() ]
        return metadata
    except AttributeError:
        return None


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
