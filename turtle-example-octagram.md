---
title: turtle example octagram (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'octagram'

Functions in program: 
* `def star(length,sides,multiple):`

## python octagram

Python turtle example: octagram

```python
# from buggy import *
from turtle import *

def star(length,sides,multiple):
    for i in range(sides):
        forward (length)
        angle = 360*multiple/sides
        right(angle)

pendown()
star(300,8,3)

done()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
