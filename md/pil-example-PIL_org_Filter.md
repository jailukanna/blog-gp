---
title: pil example PIL org Filter (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'PIL org Filter'


## python PIL org Filter

Python pil example: PIL org Filter

```python
#图片增强：加强滤镜
from __future__ import print_function
from PIL import Image,ImageFilter


imin = "test.jpg"
im = Image.open(imin)
im.show()
out = im.filter(ImageFilter.DETAIL)
out.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
