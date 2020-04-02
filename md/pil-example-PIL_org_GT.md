---
title: pil example PIL org GT (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'PIL org GT'


## python PIL org GT

Python pil example: PIL org GT

```python
#几何变换
from __future__ import print_function
from PIL import Image


imin = "test.jpg"
im = Image.open(imin)

out = im.resize((240, 240))#尺寸修改
out.show()
out = im.rotate(90) # 顺时针角度表示
out.show()

#置换
out = im.transpose(Image.FLIP_LEFT_RIGHT)
out.show()
out = im.transpose(Image.FLIP_TOP_BOTTOM)
out.show()
out = im.transpose(Image.ROTATE_180)
out.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
