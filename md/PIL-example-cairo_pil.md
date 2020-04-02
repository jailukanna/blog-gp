---
title: PIL example cairo pil (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'cairo pil'


Modules used in program: 
* `import numpy as np`
* `import cairo`

## python cairo pil

Python PIL example: cairo pil

```python
import cairo
import numpy as np
from PIL import Image

# imdata is a 2D numpy array of dtype np.uint8 containing grayscale pixel intensities on [0, 255]
# repeat for each of R, G, B, and add a deck of 255s for alpha
cairo_imdata = np.dstack([imdata, imdata, imdata, np.ones_like(imdata)*255])
surface = cairo.ImageSurface.create_for_data(cairo_imdata, cairo.FORMAT_ARGB32, *(reversed(imdata.shape)))

# create a context and do some doodling
ctx = cairo.Context(surface)
ctx.set_source_rgb(1.0, 0.0, 0.0) # pure red
ctx.set_line_width(3)
# draw an arc centered at (10,10) with radius 5, from 0 to 2*pi radians
ctx.arc(10, 10, 5, 0, 2*np.pi)
ctx.stroke()

h, w = cairo_imdata.shape[:-1]
pil_image = Image.frombuffer("RGBA", (w, h), surface.get_data(), "raw", "BGRA", 0, 1)

# now you can do PIL things
pil_image.save("image.jpg")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
