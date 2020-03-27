---
title: turtle example moving square (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'moving square'

Functions in program: 
* `def drawSquare(side):`

## python moving square

Python turtle example: moving square

```python
from turtle import *

def drawSquare(side):
    for i in range(4):
        fd(side)
        lt(90)

tracer(0)
pu(), goto(-300, 0), pd()
ht(), width(5)

count = 40000

while count > 0:
    clear()
    drawSquare(100)
    update()
    fd(0.02)
    count -= 1

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
