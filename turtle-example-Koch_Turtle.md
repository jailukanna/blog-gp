---
title: turtle example Koch Turtle (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Koch Turtle'

Functions in program: 
* `def draw_koch_snowflake(t, teragon, length):`
* `def draw_koch_curve(t, n, length):`

## python Koch Turtle

Python turtle example: Koch Turtle

```python
"""
Draw a Koch Snowflake with Turtle graphics

Tested at https://repl.it/languages/python_turtle

Koch Snowflake - https://en.wikipedia.org/wiki/Koch_snowflake
Turtle Graphics - https://en.wikipedia.org/wiki/Turtle_graphics
"""

from turtle import Turtle
from math import log

def draw_koch_curve(t, n, length):
  """Draw a Koch Curve

  t - a Turtle
  n - iteration depth (teragon)
  length - the length of the line"""
  
  if n < 1:
    # ____
    t.forward(length)
  else:
    # _/\_
    n = n - 1
    # the length of each segment is 1/3 of the length of the line
    length = length / 3
    draw_koch_curve(t, n, length)
    t.left(60)
    draw_koch_curve(t, n, length)
    t.right(120)
    draw_koch_curve(t, n, length)
    t.left(60)
    draw_koch_curve(t, n, length)

def draw_koch_snowflake(t, teragon, length):
  """Draw a Koch Snowflake
  
  t - a Turtle
  teragon - the number of iterations in a Koch Curve
  length - the length of the line of a Koch Curve"""

  # offset from the center
  t.penup()
  t.goto(-length/2, length/3)
  t.pendown()

  # draw a "triangle" where each side is a Koch Curve to make a snowflake
  for _ in range(3):
    draw_koch_curve(t, teragon, length)
    t.right(120)

# The chosen length of one side of the Koch Snowflake
# note: evenly divisible by 3, 6 times
length = 3 ** 6
# the number of iterations or "depth" of the recursion
# the power that length was raised to, that is, 6 for the chosen length
# recursing deeper than this isn't visibly noticeable on the canvas, as
# the length would be less than a pixel in the base case
teragon = int(log(length, 3))

t = Turtle()
t.speed('fastest')
draw_koch_snowflake(t, teragon, length)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
