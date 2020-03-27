---
title: turtle example turtlePlay (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtlePlay'


## python turtlePlay

Python turtle example: turtlePlay

```python
from turtle import *

if __name__ == '__main__':
	# reset the screen
	reset()
prompt = '>'
print("how many leaves do you want?")

leaf = int(input(prompt))

tanuki = Turtle()
tanuki.color(0.8, 0.5, 0.1)
tanuki.goto(256, 256)
for i in range(leaf):
	for i in range(2):
		tanuki.forward(100)
		tanuki.right(60)
		tanuki.forward(100)
		tanuki.right(120)
	tanuki.right(360/leaf)
	
	


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
