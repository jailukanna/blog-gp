---
title: turtle example trees (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'trees'

Functions in program: 
* `def main():`
* `def tree(depth,length):`

Modules used in program: 
* `import random`

## python trees

Python turtle example: trees

```python
#!/usr/bin/python
"""
trees.py

Recursion!
"""

from turtle import *
import random

def tree(depth,length):
	if depth > 0:
		#edit the following randrange ranges to generate more wild trees
		#to specify a single angle, 45 for example, enter (45,46)
		angle1 = random.randrange(10,30)
		angle2 = random.randrange(10,30)
		forward(length)
		right(angle1)
		tree(depth-1,3*length/4)
		left(angle1)
		left(angle2)
		tree(depth-1,3*length/4)
		right(angle2)
		back(length)

def main():
	depth = int(input("Enter the depth of recursion: "))
	length = int(input("Enter the initial length of the tree: "))
	speed(0)
	hideturtle()
	up()
	left(90)
	back(100)
	down()
	tree(depth,length)
	raw_input("Press enter to quit!")
	bye()

if __name__ == "__main__":
	main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
