---
title: turtle example arm (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'arm'

Functions in program: 
* `def arm(angle):`

## python arm

Python turtle example: arm

```python
from turtle import *
 
length = 190
 
def arm(angle):
    left(angle)
    forward(length)
 
speed('slow')

a = 45
b = -2*a
 
arm(a)
arm(b)

from math import *

w = cos(radians(a))*length
reach = 2*w
print(reach)

penup()
goto(reach,0)
pendown()
dot(10)
 
done()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
