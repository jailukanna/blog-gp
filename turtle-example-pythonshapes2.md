---
title: turtle example pythonshapes2 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'pythonshapes2'

Functions in program: 
* `def polygon(number_of_sides, side_length, color):`

## python pythonshapes2

Python turtle example: pythonshapes2

```python
from turtle import *

def polygon(number_of_sides, side_length, color):
	penup()
	forward(2)
	pencolor(color)
	pendown()
	for i in range (6):
		forward(2)
		right(360/ number_of_sides)
		for i in range (6):
			forward(side_length)
			right(360/ number_of_sides)

polygon(6, 50,"yellow")


getscreen()._root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
