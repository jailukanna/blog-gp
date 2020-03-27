---
title: turtle example turtle race (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtle race'

Functions in program: 
* `def myturtles(color, start, end):`

## python turtle race

Python turtle example: turtle race

```python
# code for a course Im doing
# https://www.futurelearn.com/courses/object-oriented-principles/6

from turtle import Turtle
from random import randint

def myturtles(color, start, end):
  tur = Turtle()
  tur.color(color)
  tur.shape('turtle')
  tur.penup()
  tur.goto(start, end)
  tur.pendown()
  return tur

mytur = []

mytur.append(myturtles('red', -160, 100))
mytur.append(myturtles('blue', -160, 70))
mytur.append(myturtles('purple', -160, 40))
mytur.append(myturtles('pink', -160, 10))

for movement in range(100):
  for tur in mytur:
    tur.forward(randint(1,5))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
