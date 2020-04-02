---
title: PIL example bench (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'bench'

Functions in program: 
* `def benchmark():`
* `def pil_frombytes(im):`
* `def pil_frombytes_rgb(im):`
* `def numpy_slice(im):`
* `def numpy_flip(im):`
* `def mss_rgb(im):`

Modules used in program: 
* `import mss`
* `import numpy`
* `import time`

## python bench

Python PIL example: bench

```python
# coding: utf-8
from __future__ import print_function

import time

import numpy
from PIL import Image

import mss


def mss_rgb(im):
    return im.rgb


def numpy_flip(im):
    frame = numpy.array(im, dtype=numpy.uint8)
    return numpy.flip(frame[:, :, :3], 2).tobytes()


def numpy_slice(im):
    return numpy.array(im, dtype=numpy.uint8)[..., [2, 1, 0]].tobytes()


def pil_frombytes_rgb(im):
    return Image.frombytes('RGB', im.size, im.rgb).tobytes()


def pil_frombytes(im):
    return Image.frombytes('RGB', im.size, im.bgra, 'raw', 'BGRX').tobytes()


def benchmark():
    with mss.mss() as sct:
        im = sct.grab(sct.monitors[0])
        for func in (pil_frombytes_rgb,
                     pil_frombytes,
                     mss_rgb,
                     numpy_flip,
                     numpy_slice):
            count = 0
            start = time.time()
            while (time.time() - start) <= 1:
                func(im)
                im._ScreenShot__rgb = None
                count += 1
            print(func.__name__.ljust(17), count)


benchmark()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
