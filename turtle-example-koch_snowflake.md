---
title: turtle example koch snowflake (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'koch snowflake'

Functions in program: 
* `def drawSnowflake(size, depth):`
* `def drawLine(turtle, size, depth, cur = 1):`

## python koch snowflake

Python turtle example: koch snowflake

```python
from turtle import Turtle

def drawLine(turtle, size, depth, cur = 1):
	if (cur < depth):
		drawLine(turtle, size, depth, cur + 1)
		turtle.left(60)
		drawLine(turtle, size, depth, cur + 1)
		turtle.right(120)
		drawLine(turtle, size, depth, cur + 1)
		turtle.left(60)
		drawLine(turtle, size, depth, cur + 1)
	else:
		turtle.forward(size/(3**depth))

def drawSnowflake(size, depth):
    t = Turtle()
    t.speed(0)
    for i in range(3):
        drawLine(t, size, depth)
        t.right(120)
        
drawSnowflake(1000, 4)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
