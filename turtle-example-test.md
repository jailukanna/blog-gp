---
title: turtle example test (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'test'

Functions in program: 
* `def random_xy_coord():`
* `def random_length():`
* `def draw_star(x, y, col, side):`

Modules used in program: 
* `import colorsys`
* `import random as r`

## python test

Python turtle example: test

```python
from turtle import *
import random as r
import colorsys
from math import sin, pi

def draw_star(x, y, col, side):
    ds = Turtle()
    ds.speed('fastest')
    ds.color(col)
    ds.begin_fill()
    ds.penup()
    ds.goto(x, y)
    ds.pendown()
    for k in range(5):
        ds.forward(side)
        ds.right(144)
        ds.forward(side)
    ds.end_fill()
def random_length():
    return r.randrange(5, 25)
def random_xy_coord():
    return r.randrange(-400, 400), r.randrange(-300, 300) 

bg = Turtle()
bg.screen.bgcolor('black')

colors = ['red', 'orange', 'magenta', 'green', 'blue', 'yellow', 'white']

stars = 50
for k in range(stars):
    color = r.choice(colors)
    side = random_length()
    x, y  = random_xy_coord()
    draw_star(x, y, color, side)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
