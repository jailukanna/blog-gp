---
title: turtle example Tesselation (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Tesselation'

Functions in program: 
* `def trianglecut():`

## python Tesselation

Python turtle example: Tesselation

```python
from turtle import *


def trianglecut():
    pendown()
    fd(40)
    rt(120)
    fd(40)
    rt(-60)
    fd(40)
    rt(180)
    penup()
    fd(80)
    pendown()
    fd(40)
    rt(120)
    fd(120)
    rt(120)
    fd(120)
    rt(180)
    penup()
    fd(120)
    rt(60)
    pendown()
    rt(240)
    fd(20)
    rt(180)
    penup()
    fd(20)
    pendown()
    fd(20)
    rt(-120)
    fd(40)
    rt(-120)
    fd(40)
    rt(180)

    
speed('fastest')
penup()
goto(-220,-220)
pendown()
for i in range(10):
    trianglecut()
penup()
goto(640,212)
rt(180)
fd(-90)
pendown()
for i in range(10):
    trianglecut()
penup()
goto(671,316)
pendown()
for i in range(10):
    trianglecut()
rt(180)
penup()
rt(270)
print(position())
goto(-279,-206)
fd(90)
rt(-270)
for i in range(10):
    trianglecut()
rt(180)
penup()
goto(610,420)
for i in range(10):
    trianglecut()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
