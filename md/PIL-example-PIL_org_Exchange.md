---
title: PIL example PIL org Exchange (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'PIL org Exchange'


## python PIL org Exchange

Python PIL example: PIL org Exchange

```python
#图片格式转换
from __future__ import print_function
from PIL import Image

im = "test.jpg"
imout = "test.png"
try:
    Image.open(im).save(imout)
except IOError:
    print("cannot convert", im)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
