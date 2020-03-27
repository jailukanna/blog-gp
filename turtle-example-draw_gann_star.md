---
title: turtle example draw gann star (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'draw gann star'

Functions in program: 
* `def shape(size, sides):`

Modules used in program: 
* `import math`
* `import turtle`

## python draw gann star

Python turtle example: draw gann star

```python
import turtle
from turtle import *
import math

land_window = Screen()
colormode(255)
speed(10)
pensize(3)

def shape(size, sides):
    for i in range(sides):
        fd(size)
        rt(144)
j = 50 * 0.9563048

for i in range(1, 100):
    shape(2 * j * i + i, 5)
    lt(180 - 17)
    pu()
    fd(50)
    pd()
    rt(180 - 17)

#shape(2 * j * i + i, 5)

land_window.exitonclick()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
