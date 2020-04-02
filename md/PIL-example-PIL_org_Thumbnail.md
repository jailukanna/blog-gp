---
title: PIL example PIL org Thumbnail (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'PIL org Thumbnail'


## python PIL org Thumbnail

Python PIL example: PIL org Thumbnail

```python
#缩略图
from __future__ import print_function
from PIL import Image

size = (60, 60)

imin = "test.jpg"
imout = "test.thumbnail"

try:
    im = Image.open(imin)
    im.show()
    im.thumbnail(size)
    im.save(imout, "JPEG")
except IOError:
    print("cannot create thumbnail for", imin)

im.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
