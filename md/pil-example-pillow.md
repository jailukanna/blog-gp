---
title: pil example pillow (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'pillow'

Functions in program: 
* `def load_image_from_file_path(file_path):`
* `def load_image_from_bytes(image_data):`

Modules used in program: 
* `import io`

## python pillow

Python pil example: pillow

```python
from PIL import Image

import io

def load_image_from_bytes(image_data):
    """ 从图片流中读取一张照片 """
    im = Image.open(io.BytesIO(image_data))
    print(im)
    # >>> <PIL.PngImagePlugin.PngImageFile image mode=RGBA size=300x240 at 0x102092B10>
 
def load_image_from_file_path(file_path):
    """ 从路径中读取一张图片 """
    im = Image.open(file_path)
    print(im)
     # >>> <PIL.PngImagePlugin.PngImageFile image mode=RGBA size=300x240 at 0x102092B10>

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
