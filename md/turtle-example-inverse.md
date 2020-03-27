---
title: turtle example inverse (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'inverse'

Functions in program: 
* `def arm(angle):`

## python inverse

Python turtle example: inverse

```python
from turtle import *
from math import *

length = 190
target = 260
 
def arm(angle):
    left(angle)
    forward(length)
 
speed('slow')

a = degrees(acos(0.5*target/length))
b = -2*a
print(a)
 
arm(a)
arm(b)

penup()
goto(target,0)
pendown()
dot(10)
 
done()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
