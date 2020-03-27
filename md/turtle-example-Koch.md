---
title: turtle example Koch (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Koch'

Functions in program: 
* `def koch(a, order):`

## python Koch

Python turtle example: Koch

```python
# Draw a Koch snowflake
from turtle import *

def koch(a, order):
    if order > 0:
        for t in [60, -120, 60, 0]:
            koch(a/3, order-1)
            left(t)
        
    else:
        forward(a)

# Test
koch(100, 0)
pensize(3)
koch(100, 1)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
