---
title: turtle example sierpinski triangle (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'sierpinski triangle'

Functions in program: 
* `def sierpinski(leng=150, depth=3, angle=0):`
* `def points(leng, off=(0, 0), rot=0, ang=60):`

## python sierpinski triangle

Python turtle example: sierpinski triangle

```python
from turtle import Screen, Turtle, mode, Vec2D


win = Screen()
win.bgcolor('black')
mode('logo')
tur = Turtle()
tur.shape('arrow')
tur.shapesize(0.5, 0.5, 0.5)
tur.color('lime', 'green')
tur.speed(10)

def points(leng, off=(0, 0), rot=0, ang=60):
  tur.pu()
  yield Vec2D(0, 0) + off
  tur.pd()
  yield Vec2D(0, leng).rotate(ang/2 + rot) + off
  yield Vec2D(0, leng).rotate(-ang/2 + rot) + off
  yield Vec2D(0, 0) + off

def sierpinski(leng=150, depth=3, angle=0):
  tur.fill(True)
  for i, p in enumerate(points(leng, tur.pos(), angle)):
    tur.goto(p)
    if depth > 1 and i < 3:
      sierpinski(leng/2, depth - 1, i*240 + angle)
  tur.fill(False)

sierpinski(300, 6, 180)
win.exitonclick()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
