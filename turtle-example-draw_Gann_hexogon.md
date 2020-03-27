---
title: turtle example draw Gann hexogon (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'draw Gann hexogon'

Functions in program: 
* `def shape(size, sides):`

Modules used in program: 
* `import turtle`

## python draw Gann hexogon

Python turtle example: draw Gann hexogon

```python
import turtle
from turtle import *

land_window = Screen()

pensize(3)

def shape(size, sides):
    for i in range(sides):
        fd(size)
        rt(60)

for i in range(1, 100):
    shape(20 * i, 6)
    lt(120)
    pu()
    fd(20)
    pd()
    rt(120)


land_window.exitonclick()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
