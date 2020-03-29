---
title: pygtk example blur (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'blur'

Functions in program: 
* `def blur_1d(in_pixels, out_pixels, width, height, radius):`
* `def blur_2d(in_pixels, out_pixels, width, height, radius):`
* `def blur(in_filename, out_filename, radius=10):`
* `def make_kernel(radius):`

Modules used in program: 
* `import numpy as np`
* `import gtk`
* `import pygtk`

## python blur

Python pygtk example: blur

```python
from math import sqrt, exp, pi

import pygtk
pygtk.require('2.0')

import gtk
import numpy as np

def make_kernel(radius):
  width = radius * 2 + 1
  kernel = np.zeros(width) # array with len(rows)

  sigma = radius / 3
  norm = 1 / (sqrt(2 * pi) * sigma)
  coeff = 2 * sigma ** 2

  total = 0
  for x in range(-radius, radius + 1):
    g = norm * exp(-(x ** 2) / coeff)
    kernel[x + radius] = g
    total += g;

  for i in range(width):
    kernel[i] /= total

  return kernel

def blur(in_filename, out_filename, radius=10):
  pixbuf = gtk.gdk.pixbuf_new_from_file(in_filename)
  in_pixels = pixbuf.get_pixels_array().copy()
  out_pixels = in_pixels.copy()

  width = pixbuf.get_width()
  height = pixbuf.get_height()
  
  blur_2d(in_pixels, out_pixels, width, height, radius)

  out_pixbuf = gtk.gdk.pixbuf_new_from_array(in_pixels, gtk.gdk.COLORSPACE_RGB, 8)
  return out_pixbuf.save(out_filename, 'jpeg', {
    "quality": "100"
  })

def blur_2d(in_pixels, out_pixels, width, height, radius):
  blur_1d(in_pixels, out_pixels, width, height, radius)
  out_pixels = np.transpose(out_pixels, (1, 0, 2))
  in_pixels = np.transpose(in_pixels, (1, 0, 2))
  blur_1d(out_pixels, in_pixels, height, width, radius)

def blur_1d(in_pixels, out_pixels, width, height, radius):
  kernel = make_kernel(radius)

  for j in range(0, height):
    for i in range(0, width):
      out_r, out_g, out_b = (0, 0, 0)

      for k in range(-radius, radius + 1):
        f = kernel[radius + k]
        if f != 0:
          ik = i + k

          if ik < 0:
            ik = 0
          elif ik >= width:
            ik = width - 1

          in_r, in_g, in_b = in_pixels[j][ik]

          out_r += f * in_r
          out_g += f * in_g
          out_b += f * in_b

      out_pixels[j][i] = (out_r, out_g, out_b)

  return out_pixels


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
