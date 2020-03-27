---
title: turtle example true tree (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'true tree'

Functions in program: 
* `def draw_tree(pen, x0, y0, a0, ad, l, level, init_color):`
* `def get_random_length_factor():`
* `def get_random_angle():`

Modules used in program: 
* `import math`
* `import random`

## python true tree

Python turtle example: true tree

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

import random
import math
from turtle import *

def get_random_angle():
    return random.uniform(-(math.pi * .25), math.pi * .25)

def get_random_length_factor():
    return random.uniform(0.6, 0.9)

def draw_tree(pen, x0, y0, a0, ad, l, level, init_color):
    queue = []
    counter = 2 ** level
    queue.append((x0, y0, a0 + ad, l, init_color))
    while len(queue) != 0:
        (x, y, alpha, l, color) = queue.pop(0)
        pen.up()
        pen.setpos(x, y)
        pen.pencolor(color)
        pen.setheading(alpha)
        pen.down()
        pen.forward(l)
        (x0, y0) = pen.position()
        a0 = pen.heading()
        if(counter != 0):
            counter -= 2
            (r, g, b) = color
            r *= 1.1
            g *= 1.1
            b *= 1.1
            color = (math.floor(r), math.floor(g), math.floor(b))
            queue.append((x0, y0, a0 + get_random_angle(), l * get_random_length_factor(), color))
            queue.append((x0, y0, a0 - get_random_angle(), l * get_random_length_factor(), color))

if __name__ == '__main__':
    screensize(1600, 800)
    colormode(255)
    bgcolor('black')
    pen = Turtle()
    pen.radians()
    pen.speed(10)
    draw_tree(pen, 0, -400, math.pi/4, math.pi/4, 200, 10, (0,100,0)) 
    done()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
