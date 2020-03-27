---
title: turtle example sample (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'sample'

Functions in program: 
* `def make_circle(radius, N):`
* `def draw_circle(i, N, length):`
* `def init(max):`

## python sample

Python turtle example: sample

```python
# Problem: A Perfect Circle
# Author: Sean Strout (sps@cs.rit.edu)
# Description: Draw a circle as a series of line segments.  The circle should
# be drawn at the center of the canvas with a radius of 5 units and 100 line
# segments.

from turtle import *
from math import *

def init(max):
	"""Initialize the world with (0,0) in the center.
		max - the max extent of the world"""
	setworldcoordinates(-max, -max, max, max)
	
	# set pen thickness
	pensize(3)
	
def draw_circle(i, N, length):
	"""Draw a segment of the circle.
		i - current segment
		N - the maximum number of segments
		length - the length of the segment
	"""	
	# terminate when all the segments have been drawn	
	if (i == N):
		return
		
	# draw this segment
	forward(length)
	
	# turn left
	left(360 / N)
	
	# draw next segment
	draw_circle(i+1, N, length)
	
def make_circle(radius, N):
	"""Draw a circle with a supplied radius.
		radius - the radius of the circle
		N - the number of line segments used to draw the circle
	"""
	# compute length for each line segment of the circle
	circumference = 2 * pi * radius
	length = circumference / N
	
	# position turtle at bottom middle of circle
	up()
	right(90)
	forward(radius)
	left(90)
	down()
	
	# draw the circle
	draw_circle(0, N, length)
	
	# reposition turtle at center of circle
	up()
	left(90)
	forward(radius)
	right(90)
	down()


# circle radius and # segments
RADIUS = 5
SEGMENTS = 100

# initialize the scene	
init(RADIUS+1)	

# draw the circle
make_circle(RADIUS, SEGMENTS)

# wait for input to close
input("Hit enter to exit.")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
