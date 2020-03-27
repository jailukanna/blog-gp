---
title: turtle example cool-circle (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'cool-circle'

Functions in program: 
* `def draw():`

## python cool-circle

Python turtle example: cool-circle

```python
from turtle import *
from time import *
rotate=1
rotate2=180
repeat=1
tracer(False)
hideturtle()
def draw():
    circle(100)
while True:
    for i in range(0, 100):
        for i in range(0, repeat):
            draw()
            right(rotate2)
            update()
        rotate2=rotate2/rotate
        rotate=rotate + 6
        repeat=repeat  * 50



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
