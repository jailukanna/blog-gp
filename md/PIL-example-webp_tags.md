---
title: PIL example webp tags (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'webp tags'

Functions in program: 
* `def webp(image):`

## python webp tags

Python PIL example: webp tags

```python
from io import BytesIO

from django import template
from django.core.files import File

from webp import WebPPicture, WebPConfig, WebPPreset
from PIL import Image


register = template.Library()


@register.filter(name='webp')
def webp(image):
    name = '{path}.wepb'.format(path=image.name.split('.')[0])
    if image.storage.exists(name):
        return image.storage.url(name)

    with image.storage.open(image.name) as imgfile:
        img = Image.open(imgfile)
        webpic = WebPPicture.from_pil(img)
        config = WebPConfig.new(preset=WebPPreset.PHOTO, quality=84)
        buff = webpic.encode(config).buffer()
        image.storage.save(name, BytesIO(buff))
    return image.storage.url(name)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
