---
title: turtle example SkeletalTree (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'SkeletalTree'

Functions in program: 
* `def tree(d, n):`

## python SkeletalTree

Python turtle example: SkeletalTree

```python
from turtle import *
speed(0), ht()


diff, a = 0.6, 45

def tree(d, n):
	'this function uses recursion to draw'
	if n > 1:
		fd(d)
		lt(a)
	
		tree(d*diff, n-1) 
		
		rt(2*a)
		
		tree(d*diff, n-1)
		
		lt(a), bk(d)
		
lt(90), bk(100)

tree(100, 8)

exitonclick()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
