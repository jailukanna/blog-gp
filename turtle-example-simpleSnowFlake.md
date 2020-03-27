---
title: turtle example simpleSnowFlake (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'simpleSnowFlake'


## python simpleSnowFlake

Python turtle example: simpleSnowFlake

```python
from turtle import *


pensize(5)
pencolor('skyblue')
branches = int(input("How many branches?"))
eq = 360/branches
for count in range(branches):
  forward(100)
  #first branch
  left(45)
  forward(50)
  backward(50)
  #second branch
  right(90)
  forward(50)
  backward(50)
  #mid branch
  left(45)
  forward(50)
  backward(150)
  #start new branch
  left(eq)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
