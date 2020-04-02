---
title: PIL example resize (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'resize'

Functions in program: 
* `def get_new_path(path): `

Modules used in program: 
* `import re, os, sys, urlparse`
* `import PIL.Image as PIL`

## python resize

Python PIL example: resize

```python
#! /usr/bin/env python

"""
via https://pandemoniumillusion.wordpress.com/2008/05/04/quick-image-resizing-with-python/
What:
Resize all the jpg images in a directory
All images will be placed inside a directory called "resized"
This directory must exist
 

"""
import PIL.Image as PIL
import re, os, sys, urlparse

SIZE = 650,650
JPG = re.compile(".*\.(jpg|jpeg)", re.IGNORECASE)

def get_new_path(path): 
    basedir = os.path.dirname(path) + '/resized/'
    base, ext = os.path.splitext(os.path.basename(path))
    file = base + ext

    return urlparse.urljoin(basedir, file)

try:
    image_dir = sys.argv[1]
except:
    print("You must specify a directory")
    exit(0)

files = os.listdir(image_dir)
for file in files:
    if JPG.match(file):
        f = image_dir.rstrip("/") + "/" + file
        if 'square' in f:
            print(f)
            img = PIL.open(f)
            img.thumbnail(SIZE, PIL.ANTIALIAS)
            img.save(get_new_path(f))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
