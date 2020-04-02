---
title: pil example imagehash phash lenna (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'imagehash phash lenna'


Modules used in program: 
* `import imagehash`
* `import PIL`

## python imagehash phash lenna

Python pil example: imagehash phash lenna

```python
# Code for the blogpost https://fullstackml.com/2016/07/02/wavelet-image-hash-in-python/
import PIL
from PIL import Image
import imagehash

lenna = PIL.Image.open(‘lenna.png’)
lenna1 = PIL.Image.open(‘lenna1.jpg’)

h = imagehash.phash(lenna)
h1 = imagehash.phash(lenna1)
h-h1
# > 0

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
