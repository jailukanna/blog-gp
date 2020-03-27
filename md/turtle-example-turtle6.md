---
title: turtle example turtle6 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtle6'

Functions in program: 
* `def branch(length):`

## python turtle6

Python turtle example: turtle6

```python
from turtle import *

def branch(length):
	"""
	長さを引数として受け取って枝を描く
	"""
	if length < 10:
		return
	forward(length)
	left(30)
	branch(length / 2)
	right(60)
	branch(length / 2)
	left(30)
	forward(-length)

branch(200)

input()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
