---
title: PIL example PIL org Split (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'PIL org Split'


## python PIL org Split

Python PIL example: PIL org Split

```python
#通道分离
from __future__ import print_function
from PIL import Image


imin = "testbig.jpg"
im = Image.open(imin)


r, g, b = im.split()#通道分离
r.show()
g.show()
b.show()

im = Image.merge("RGB", (b, g, r))#b,r通道翻转后合并
im.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
