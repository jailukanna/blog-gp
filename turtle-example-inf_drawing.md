---
title: turtle example inf drawing (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'inf drawing'


## python inf drawing

Python turtle example: inf drawing

```python
from turtle import *
from random import randint

pcb = ['red', 'pink', 'yellow', 'green', 'blue']
bgcolor('black')
color(pcb[0])
pensize(1)
speed('fastest')
penup()
# setpos(-400, 0)
pendown()

for i in range(2000):
    color(pcb[randint(0, 4)])
    dot(5)
    left(randint(0, 360))
    forward(20)
    right(randint(0, 360))

done()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
