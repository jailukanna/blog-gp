---
title: PIL example sepia image processor (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'sepia image processor'


## python sepia image processor

Python PIL example: sepia image processor

```python
"""
Adds a sepia filter processor for django-imagekit which
can be used in an imagekit specification.

https://github.com/jdriscoll/django-imagekit

See http://effbot.org/zone/pil-sepia.htm for the original
code for the sepia filtering in PIL

"""
from PIL import ImageOps
from imagekit.processors import ImageProcessor


class Sepia(ImageProcessor):
    """
        Turn image sepia
    """
    @classmethod
    def process(cls, img, fmt, obj):

        def make_linear_ramp(white):
            # putpalette expects [r,g,b,r,g,b,...]
            ramp = []
            r, g, b = white
            for i in range(255):
                ramp.extend((r*i/255, g*i/255, b*i/255))
            return ramp

        img = img.convert('RGB')
        sepia = make_linear_ramp((255, 240, 192))
        if img.mode != "L":
            img = img.convert("L")
        img = ImageOps.autocontrast(img)
        img.putpalette(sepia)
        img = img.convert('RGB')
        fmt = "JPEG"
        return img, fmt


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
