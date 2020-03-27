---
title: turtle example daisy (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'daisy'

Functions in program: 
* `def flower():`
* `def petal():`

## python daisy

Python turtle example: daisy

```python
#!/usr/bin/python3

from turtle import *

biro = Turtle()
biro.color("purple", "yellow")
biro.speed(0)

def petal():
	biro.begin_fill()
	biro.right(135)
	biro.circle(50,90)
	biro.left(90)
	biro.circle(50,90)
	biro.right(135)
	biro.end_fill()

def flower():
	for petals in range(12):
		petal()
		biro.circle(20,30)

biro.goto(-500,0)

for flowers in range(12):
	flower()
	biro.forward(100)

exitonclick()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
