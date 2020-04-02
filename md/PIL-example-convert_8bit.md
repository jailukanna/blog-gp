---
title: PIL example convert 8bit (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'convert 8bit'

Functions in program: 
* `def quantizetopalette(silf, palette, dither=False):`

Modules used in program: 
* `import numpy as np`
* `import PIL`

## python convert 8bit

Python PIL example: convert 8bit

```python
import PIL
import numpy as np

def quantizetopalette(silf, palette, dither=False):
    """Convert an RGB or L mode image to use a given P image's palette."""

    silf.load()

    # use palette from reference image
    palette.load()
    if palette.mode != "P":
        raise ValueError("bad mode for palette image")
    if silf.mode != "RGB" and silf.mode != "L":
        raise ValueError(
            "only RGB or L mode images can be quantized to a palette"
            )
    im = silf.im.convert("P", 1 if dither else 0, palette.im)
    # the 0 above means turn OFF dithering
    return silf._makeself(im)
	
img = PIL.Image.open('in.bmp').convert('RGB')
pal = PIL.Image.open('palette.bmp').convert('P')

img = quantizetopalette(img, pal, dither=False)

img.save('out.bmp')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
