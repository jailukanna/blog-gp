---
title: turtle example sierpinski-triangle (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'sierpinski-triangle'

Functions in program: 
* `def sierpinski(length,depth):`
* `def sierpinskiInner(length,depth):`
* `def side(length,depth):`

## python sierpinski-triangle

Python turtle example: sierpinski-triangle

```python
from turtle import *

def side(length,depth):
    forward(length/2)
    if depth>0:
        left(120)
        sierpinskiInner(length/2,depth-1)
        left(120)
    forward(length/2)

def sierpinskiInner(length,depth):
    side(length,depth)
    right(120)
    side(length,depth)
    right(120)
    side(length,depth)

def sierpinski(length,depth):
    right(60)
    forward(length)
    right(120)
    forward(length)
    right(120)
    forward(length/2)
    if depth>0:
        right(60)
        sierpinskiInner(length/2,depth-1)
        right(60)
    forward(length/2)
    right(60)

speed('fastest')
penup()
goto(0,200)
pendown()

sierpinski(500,4)

done()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
