---
title: turtle example binary tree (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'binary tree'

Functions in program: 
* `def binary_tree(length, angle, depth):`

## python binary tree

Python turtle example: binary tree

```python
from turtle import *

def binary_tree(length, angle, depth):
    pendown()
    forward(length)
    left(angle)
    if depth > 0:
        binary_tree(length/2, angle, depth - 1)
    right(angle*2)
    if depth > 0:
        binary_tree(length/2, angle, depth - 1)
    left(angle)
    forward(-length)
    penup()
    
binary_tree(100,30,2)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
