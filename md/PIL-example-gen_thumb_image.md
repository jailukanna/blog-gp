---
title: PIL example gen thumb image (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'gen thumb image'

Functions in program: 
* `def gen_thumb_image(path, width=0, height=0, filetype='JPEG'):`

## python gen thumb image

Python PIL example: gen thumb image

```python
from PIL import Image as ImagePIL

def gen_thumb_image(path, width=0, height=0, filetype='JPEG'):
    '''
    生成缩略图
    '''
    width = min(1024, width)
    height = min(1024, height)
    img = ImagePIL.open(path)
    if width and not height:
        height = float(width) / img.size[0] * img.size[1]
    if not width and height:
        width = float(height) / img.size[1] * img.size[0]
    stream = BytesIO()
    img.thumbnail((width, height), ImagePIL.ANTIALIAS)
    img.save(stream, format=filetype, optimize=True)
    return stream

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
