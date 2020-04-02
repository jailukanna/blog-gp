---
title: PIL example gradient (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'gradient'

Functions in program: 
* `def getcolor(shift = 0):`

Modules used in program: 
* `import os`
* `import PIL`
* `import random`
* `import colorsys`

## python gradient

Python PIL example: gradient

```python
from ctypes.wintypes import windll, c_int, byref, RGB
from random import randint
import colorsys
import random
import PIL
from PIL import Image
import os
from os import path

w = 1
h = 2

COLOR_BACKGROUND = 1
SPI_SETDESKWALLPAPER = 0x14
SPIF_UPDATEINIFILE   = 0x01
SPIF_SENDWININICHANGE = 0x02
filename = "gradient.bmp"

hue = random.random()

def getcolor(shift = 0):
    h = hue#random.random()
    s = random.random()
    l = random.random()
    h += shift;
    if (h > 1):
        h -= 1

    rgb = colorsys.hls_to_rgb(h, l, s)
    return (int(rgb[0] * 255), int(rgb[1] * 255), int(rgb[2] * 255))

img = Image.new("RGB", (w,h))
img.putpixel((0,0), getcolor(0))
img.putpixel((0,1), getcolor(0.5))
img = img.resize((32,32), PIL.Image.ANTIALIAS)
img.save(filename);

lpszImage = os.path.join(os.getcwd(), filename)
windll.user32.SystemParametersInfoA(SPI_SETDESKWALLPAPER, 0, lpszImage, SPIF_UPDATEINIFILE | SPIF_SENDWININICHANGE)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
