---
title: pygtk example sciblur (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'sciblur'

Functions in program: 
* `def blur(in_filename, out_filename, radius):`

Modules used in program: 
* `import numpy as np`
* `import gtk`
* `import pygtk`

## python sciblur

Python pygtk example: sciblur

```python
from math import ceil

import pygtk
pygtk.require('2.0')
import gtk

import numpy as np

from scipy import ndimage

def blur(in_filename, out_filename, radius):
  in_pixbuf = gtk.gdk.pixbuf_new_from_file(in_filename)
  in_pixels = in_pixbuf.get_pixels_array().copy()

  r_pixels, g_pixels, b_pixels = np.dsplit(in_pixels, 3)

  sigma = ceil(radius / 3)
  ndimage.gaussian_filter(r_pixels, output=r_pixels, sigma=sigma)
  ndimage.gaussian_filter(g_pixels, output=g_pixels, sigma=sigma)
  ndimage.gaussian_filter(b_pixels, output=b_pixels, sigma=sigma)

  out_pixels = np.dstack((r_pixels, g_pixels, b_pixels))
  out_pixbuf = gtk.gdk.pixbuf_new_from_array(out_pixels, gtk.gdk.COLORSPACE_RGB, 8)
  return out_pixbuf.save(out_filename, 'jpeg', {
    'quality': '100'
  })

if __name__ == '__main__':
  import timeit
  print(timeit.timeit('blur("test.jpg", "blurred.jpg", 120)', setup='from __main__ import blur', number=1))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
