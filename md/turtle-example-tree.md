---
title: turtle example tree (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'tree'

Functions in program: 
* `def draw_tree(pen, x0, y0, a0, ad, l, level):`
* `def get_random_color():`

Modules used in program: 
* `import math`
* `import random`

## python tree

Python turtle example: tree

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

import random
import math
from turtle import *

def get_random_color():
    return (random.randint(0, 255), random.randint(0, 255), random.randint(0, 255))

def draw_tree(pen, x0, y0, a0, ad, l, level):
    queue = []
    counter = 2 ** level
    queue.append((x0, y0, a0 + ad, l))
    while len(queue) != 0:
        (x, y, alpha, l) = queue.pop(0)
        pen.up()
        pen.setpos(x, y)
        pen.pencolor(get_random_color())
        pen.setheading(alpha)
        pen.down()
        pen.forward(l)
        (x0, y0) = pen.position()
        a0 = pen.heading()
        if(counter != 0):
            counter -= 2
            queue.append((x0, y0, a0 + ad, l * .75))
            queue.append((x0, y0, a0 - ad, l * .75))

if __name__ == '__main__':
    screensize(1600, 800)
    colormode(255)
    pen = Turtle()
    pen.radians()
    pen.speed(10)
    draw_tree(pen, 0, -400, math.pi/4, math.pi/4, 200, 10) 
    done()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
