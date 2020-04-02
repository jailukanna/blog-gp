---
title: pil example art (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'art'

Functions in program: 
* `def add(a, b):`
* `def linear(a, b, t):`
* `def circle(c, r, a):`

Modules used in program: 
* `import cairo`
* `import PIL.Image`
* `import PIL`
* `import sys`
* `import os`
* `import math`

## python art

Python pil example: art

```python
import math
import os
import sys

import PIL
import PIL.Image
import cairo

WIDTH, HEIGHT = 512, 512

surface = cairo.ImageSurface(cairo.FORMAT_ARGB32, WIDTH, HEIGHT)
ctx = cairo.Context(surface)

ctx.scale(WIDTH, HEIGHT)

ims = []

nr_frames = 30
nr_triangles = 90

p0 = (.5 + .4 * math.cos(0 * 120 * math.pi / 180.), .5 + .4 * math.sin(0 * 120 * math.pi / 180.))
p1 = (.5 + .4 * math.cos(1 * 120 * math.pi / 180.), .5 + .4 * math.sin(1 * 120 * math.pi / 180.))
p2 = (.5 + .4 * math.cos(2 * 120 * math.pi / 180.), .5 + .4 * math.sin(2 * 120 * math.pi / 180.))

def circle(c, r, a):
    x = c[0] + r * math.cos(a * math.pi / 180.)
    y = c[1] + r * math.sin(a * math.pi / 180.)
    return (x, y)

def linear(a, b, t):
    ti = 1 - t
    x = a[0] * t + b[0] * ti
    y = a[1] * t + b[1] * ti
    return (x, y)

def add(a, b):
    x = a[0] + b[0]
    y = a[1] + b[1]
    return (x, y)

for frame in range(nr_frames):
    ctx.set_source_rgb(1, 1, 1)
    ctx.rectangle(0, 0, 1, 1)
    ctx.fill()

    def draw_triangle(c, a):
        r = .1
        ctx.move_to(*circle(c, r, a + 0 * 120))
        ctx.line_to(*circle(c, r, a + 1 * 120))
        ctx.line_to(*circle(c, r, a + 2 * 120))
        ctx.close_path()

    for i in range(nr_triangles):
        t = 1. * i / nr_triangles
        c1 = circle((.5, .5), .3, 360. * t)
        draw_triangle(c1, 15 * math.sin(3 * math.pi * i / nr_frames) + frame * (120. / nr_frames))

        #ctx.set_source_rgba(0, 0, 0, math.sin(1. * i * math.pi / nr_triangles))
        ctx.set_source_rgb(0, 0, 0)
        ctx.set_line_width(0.003)
        ctx.stroke()

    data = surface.get_data()[:]
    im = PIL.Image.frombuffer('RGBA', (WIDTH, HEIGHT), data, 'raw', 'RGBA', 0, 1)
    ims.append(im)

ims[0].save('example.gif', save_all=True, append_images=list(ims[1:]), loop=0, duration=30)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
