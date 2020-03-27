---
title: turtle example flowerThingy p3 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'flowerThingy p3'

Functions in program: 
* `def main():`
* `def configureTurtle():`

## python flowerThingy p3

Python turtle example: flowerThingy p3

```python
from turtle import *

def configureTurtle():
	colormode(255)
	hideturtle()
	delay(1)
	speed(0)




def main():
	s = 1
	for c in range(255, 0, -1):
		color( (c, c, c) )

		for x in range(27):
			fd( (.2 * x) + ( s * .025 ) )
			left(45)

		right(60)
		fd(10+s)
		s = ( s * .005 ) + (s+0.5)

	return None


if __name__ == "__main__":
	configureTurtle()
	main()
	done()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
