---
title: turtle example graphic 1 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'graphic 1'

Functions in program: 
* `def finish_outline():`
* `def draw_circles():`
* `def draw_squares():`

## python graphic 1

Python turtle example: graphic 1

```python
'''
Playing with turtle in python 3.
'''

from turtle import *

def draw_squares():
    sides = 4
    squares = 10
    side_length = 10

    pen2.right(180)

    for i in range(squares):
        pen1.begin_fill()
        pen2.begin_fill()
        for s in range(sides):
            pen1.forward(side_length)
            pen2.forward(side_length)
            pen1.left(90)
            pen2.left(90)
        side_length+=10
        pen1.end_fill()
        pen2.end_fill()

def draw_circles():
    pen1.left(45)
    pen2.left(45)

    circle_pixel = 5
    for i in range(11):
        pen1.circle(circle_pixel)
        pen2.circle(circle_pixel)
        circle_pixel+=5

def finish_outline():
    pen1.left(45)
    pen2.left(45)
    for i in range(4):
        pen1.forward(100)
        pen2.forward(100)
        pen1.left(90)
        pen2.left(90)
    pen1.right(225)
    pen2.right(225)
    pen1.backward(50)
    pen2.backward(50)

pen1 = Pen()
pen2 = Pen()

pen1.screen.bgcolor("#302ebf")

pen1.color("#111044")
pen2.color("#1c1a6d")

draw_squares()
draw_circles()
finish_outline()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
