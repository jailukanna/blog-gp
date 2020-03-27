---
title: turtle example draftsman (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'draftsman'

Functions in program: 
* `def to_point(x,y):`
* `def on_vector(dx,dy):`
* `def pen_up():`
* `def pen_down():`
* `def test_drawman():`
* `def drawman_scale(scale):`
* `def drawman_setka():`
* `def init_drawman():`

## python draftsman

Python turtle example: draftsman

```python
from turtle import Turtle
def init_drawman():
    global t, x_current, y_corrent, _drawman_scale
    t = Turtle()
    t.penup()
    x_current = 0
    y_corrent = 0
    t.goto(x_current,y_corrent)
    drawman_scale(10)


def drawman_setka():
    t.color("grey")
    y = _drawman_scale*-5
    x = _drawman_scale*-5
    for i in range(16):
        t.goto(x, y-_drawman_scale)
        pen_down()
        t.goto(x,y*-2+_drawman_scale)
        pen_up()
        x = x +_drawman_scale
    y = _drawman_scale * -5
    x = _drawman_scale * -5
    for i in range(16):
        t.goto(x-_drawman_scale, y)
        pen_down()
        t.goto(x*-2+_drawman_scale, y)
        pen_up()
        y = y + _drawman_scale
    t.color("black")
    y = _drawman_scale * -5
    x = _drawman_scale * -5
    t.goto(x-2*_drawman_scale,0)
    pen_down()
    t.goto(x*-2+2*_drawman_scale,0)
    pen_up()
    t.goto(0,y-2*_drawman_scale)
    pen_down()
    t.goto(0,y*-2+2*_drawman_scale)
    pen_up()
    t.color("red")

def drawman_scale(scale):
    global _drawman_scale
    _drawman_scale = scale

def test_drawman():
    pen_down()
    for i in range(5):
        on_vector(10,20)
        on_vector(0,-20)
    pen_up()
    to_point(0,0)

def pen_down():
    t.pendown()

def pen_up():
    t.penup()

def on_vector(dx,dy):
    to_point(x_current + dx,y_corrent + dy)

def to_point(x,y):
    global x_current, y_corrent
    x_current=x
    y_current=y
    t.goto(_drawman_scale*x_current,_drawman_scale*y_current)


init_drawman()
if __name__ == '__main__':
    test_drawman()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
