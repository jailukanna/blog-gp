---
title: pil example PIL org ModeEx (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'PIL org ModeEx'


## python PIL org ModeEx

Python pil example: PIL org ModeEx

```python
#模式转换
from __future__ import print_function
from PIL import Image


imin = "test.jpg"
im = Image.open(imin)

cmyk = im.convert("CMYK")#C：Cyan = 青色;M：Magenta = 品红色;Y：Yellow = 黄色;K：Key Plate(blacK) = 定位套版色（黑色）
cmyk.show()
gray = im.convert("L")
gray.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
