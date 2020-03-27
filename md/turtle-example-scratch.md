---
title: turtle example scratch (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'scratch'


## python scratch

Python turtle example: scratch

```python
# There's a library called turtle, it can do serval things.
# by using the * we're saying import all of those things
from turtle import *

# The left hand side of this can be called anything you want
# you are creating you're own instance of the Turtle object
# that will probably be explained in more detail in the course
TommyTheTurtle = Turtle()

for _ in range(4):
  # when you use the .forward method, you have to put a number in the brackets. 
  # in this instance it means 100 pixels forwards
  TommyTheTurtle.forward(100)
  # in this instance it means 90 degrees clockwise
  TommyTheTurtle.right(90)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
