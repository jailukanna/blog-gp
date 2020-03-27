---
title: turtle example tortoise (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'tortoise'

Functions in program: 
* `def polygon(length, sides):`

## python tortoise

Python turtle example: tortoise

```python
# from buggy import *
from turtle import *

def polygon(length, sides):
    for i in range(sides):
        forward(length)
        angle = 360/sides
        left(angle)

pendown()

for i in range(5):
    polygon(30,5)
    forward(30)
    right(72)

done()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
