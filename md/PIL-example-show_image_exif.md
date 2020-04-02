---
title: PIL example show image exif (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'show image exif'

Functions in program: 
* `def get_exif_metadata(image_path):`
* `def decode_gps_info(exif):`

Modules used in program: 
* `import argparse`

## python show image exif

Python PIL example: show image exif

```python
#!/usr/bin/env python

import argparse
try:
    from PIL import Image
    from PIL.ExifTags import TAGS, GPSTAGS
except ImportError:
    print("You need to install PIL. Please check: http://www.pythonware.com/products/pil/")

def decode_gps_info(exif):
    gpsinfo = {}
    if 'GPSInfo' in exif:
        for key in exif['GPSInfo'].keys():
            decode = GPSTAGS.get(key,key)
            gpsinfo[decode] = exif['GPSInfo'][key]
        exif['GPSInfo'] = gpsinfo

def get_exif_metadata(image_path):
    ret = {}
    image = Image.open(image_path)
    if hasattr(image, '_getexif'):
        exifinfo = image._getexif()
        if exifinfo is not None:
            for tag, value in exifinfo.items():
                decoded = TAGS.get(tag, tag)
                ret[decoded] = value
    decode_gps_info(ret)
    return ret


if __name__ == '__main__':
    parser = argparse.ArgumentParser(description='Display exif metadata of a image if available.')
    parser.add_argument('image_path', metavar='image_path', help='Complete image path.')

    args = parser.parse_args()
    if args.image_path:
        for key, value in get_exif_metadata(args.image_path).items():
            print(key, value)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
