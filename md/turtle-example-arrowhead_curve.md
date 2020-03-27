---
title: turtle example arrowhead curve (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'arrowhead curve'

Functions in program: 
* `def curve(depth, length, angle):`

## python arrowhead curve

Python turtle example: arrowhead curve

```python
from turtle import *

def curve(depth, length, angle):
    if depth == 0:
        if angle > 0:
            pencolor('green')
        else:
            pencolor('blue')
        
        forward(length)
    else:
        curve(depth -1, length, -angle)
        right(angle)
        curve(depth -1, length, angle)
        right(angle)
        curve(depth -1, length, -angle)        

speed(0)
shape("turtle")

width(3)
curve(6, 5, 60)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
