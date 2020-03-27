---
title: turtle example p (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'p'

Functions in program: 
* `def draw_at_location(x, y):  `
* `def draw():`
* `def _circle(steps=None):`
* `def size():`
* `def random_point():`

Modules used in program: 
* `import threading`
* `import random`
* `import functools`

## python p

Python turtle example: p

```python
#!/usr/bin/env python3

import functools
import random
import threading

from turtle import *

lock = threading.Lock()

#window = Screen()
#window.screensize(2880, 1620)
#window.setup(width=1.0, height=1.0, startx=None, starty=None)

bgcolor('black')

setposition(0, 0)
_width = window_width()
_height = window_height()
dim = min(_width, _height)

width(round(dim/60))

def random_point():
    random.seed()
    return (random.randint(round(-p/2), round(p/2)) for p in (_width, _height))

def size():
    random.seed()
    return random.randint(round(dim/20), round(dim/4))

shapes = []

def _circle(steps=None):
    speed(0)
    penup()

    radius = size()

    # http://stackoverflow.com/a/24636823/228539
    right(90)    # Face South
    forward(radius)   # Move one radius
    right(270)   # Back to start heading
    pendown()
    speed('normal')
    circle(radius=radius, steps=steps)
    penup()

shapes.append(_circle)

for i in range(3,7):
    shapes.append(functools.partial(_circle, steps=i))

colors = ['red', 'green', 'blue', 'cyan', 'magenta', 'yellow', 'white']

def draw():
    draw_at_location(*random_point())

def draw_at_location(x, y):  
    if lock.acquire(timeout=0):
        speed(0)
        penup()
        setposition(x, y)
        color(random.choice(colors))
        fillcolor(random.choice(colors))
        begin_fill()
        speed('normal')
        random.choice(shapes)()
        end_fill()
        penup()

        lock.release()

onkeypress(draw)
onkeypress(clear, key='Delete')
onscreenclick(draw_at_location)
listen()

done()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
