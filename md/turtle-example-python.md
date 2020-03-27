---
title: turtle example python (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'python'

Functions in program: 
* `def drawEye(coord):`
* `def drawPython():`

## python python

Python turtle example: python

```python
from turtle import *

def drawPython():
    begin_fill()
    forward(90)
    circle(50, 90)
    forward(80)
    circle(5, 90)
    forward(140)
    circle(5, 90)
    forward(65)
    rt(90)
    forward(50)
    circle(5, 90)
    forward(105)
    circle(5, 90)
    forward(20)
    lt(90)
    forward(50)
    rt(90)
    forward(50)
    rt(180)
    circle(50, 90)
    rt(180)
    forward(50)
    rt(90)
    forward(50)
    end_fill()

def drawEye(coord):
    goto(coord)
    color('#ffffff')
    setheading(90)
    forward(15)
    rt(90)
    begin_fill()
    circle(15)
    end_fill()

speed('fastest')
color('#3776ab')
drawPython()
penup()
forward(95)
rt(90)
forward(5)
rt(90)
color('#ffbc29')
drawPython()
drawEye((15, 80))
drawEye((85, -130))
hideturtle()
done()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
