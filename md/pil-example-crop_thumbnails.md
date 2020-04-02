---
title: pil example crop thumbnails (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'crop thumbnails'

Functions in program: 
* `def resize_and_crop(img_path, modified_path, size, crop_type='top'):`

Modules used in program: 
* `import logging`
* `import glob`
* `import sys`
* `import os`

## python crop thumbnails

Python pil example: crop thumbnails

```python
# coding=utf-8
""" 
 Usage: 
    $:> python crop.py thumbs 240 180
    $:> python crop.py posters 800 600
"""

import os
import sys
import glob
import logging
from PIL import Image
from PIL import ImageOps
from PIL import ImageFilter
from PIL import ImageEnhance


logging.basicConfig(format='%(asctime)s %(message)s', level=logging.DEBUG)
log = logging.getLogger()
log.setLevel(logging.DEBUG)

domain = "eegay.com"
thumb_size = (250, 170)


def resize_and_crop(img_path, modified_path, size, crop_type='top'):
    img = Image.open(img_path)
    img_ratio = img.size[0] / float(img.size[1])
    ratio = size[0] / float(size[1])

    if ratio > img_ratio:
        img = img.resize((size[0], round(size[0] * img.size[1] / img.size[0])),
                         Image.ANTIALIAS)
        if crop_type == 'top':
            box = (0, 0, img.size[0], size[1])
        elif crop_type == 'middle':
            box = (0, round((img.size[1] - size[1]) / 2), img.size[0],
                   round((img.size[1] + size[1]) / 2))
        elif crop_type == 'bottom':
            box = (0, img.size[1] - size[1], img.size[0], img.size[1])
        else:
            raise ValueError('ERROR: invalid value for crop_type')
        img = img.crop(box)
    elif ratio < img_ratio:
        img = img.resize((round(size[1] * img.size[0] / img.size[1]), size[1]),
                         Image.ANTIALIAS)
        # Crop in the top, middle or bottom
        if crop_type == 'top':
            box = (0, 0, size[0], img.size[1])
        elif crop_type == 'middle':
            box = (round((img.size[0] - size[0]) / 2), 0,
                   round((img.size[0] + size[0]) / 2), img.size[1])
        elif crop_type == 'bottom':
            box = (img.size[0] - size[0], 0, img.size[0], img.size[1])
        else:
            raise ValueError('ERROR: invalid value for crop_type')
        img = img.crop(box)
    else:
        img = img.resize((size[0], size[1]), Image.ANTIALIAS)
    img.save(modified_path)

c = 0
size_name = sys.argv[1]
size_w = int(sys.argv[2])
size_h = int(sys.argv[3])

for file_path in glob.glob("images/2016-06-10/*.jpg"):
    dir_path, file_name = file_path.rsplit("/", 1)
    dir_path = dir_path.replace("images", size_name)

    try:
        os.makedirs(dir_path)
    except FileExistsError:
        pass

    try:
        resize_and_crop(file_path, "{}/{}".format(dir_path, file_name), (size_w, size_h), crop_type="middle")
    except OSError as e:
        log.error(e)

    c += 1
    if not c % 100:
        log.debug("> {} got".format(c))

log.debug("> {} complete".format(c))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
