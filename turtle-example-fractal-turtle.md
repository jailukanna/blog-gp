---
title: turtle example fractal-turtle (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'fractal-turtle'

Functions in program: 
* `def my_figure(d, z):`

Modules used in program: 
* `import random`

## python fractal-turtle

Python turtle example: fractal-turtle

```python
from turtle import *
import random

k = 4

def my_figure(d, z):
	step = 100/z
	kk = 4
	dd = 360/k
	
	def my_forward():
		if d!=1:
			my_figure(d-1, z*kk)
		else:
			forward(step)
	
	my_forward()
	left(dd)
	my_forward()
	right(dd)
	my_forward()
	right(dd)
	my_forward()
	my_forward()
	left(dd)
	my_forward()
	left(dd)
	my_forward()
	right(dd)
	my_forward()

	
	
	

#color('red', 'yellow')
#begin_fill()
speed(9)
pu()
setposition(-300,200)
pd()

for t in range(1, 6):
	red = random.random()
	green = random.random()
	blue = random.random()
	tup = (red, green, blue)
	pencolor(tup)
	print(tup)
	
	for i in range(k):
		my_figure(t, 1)
		right(360/k)
		
	#clear()
	
#end_fill()
done()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
