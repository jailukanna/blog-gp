---
title: turtle example turtle-demo (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtle-demo'

Functions in program: 
* `def diamond(x,y,size,step=3):`
* `def sweep(x,y,size,start,end,step):`
* `def box(x=None,y=None,size=100,angle=None):`

## python turtle-demo

Python turtle example: turtle-demo

```python
from turtle import *

speed(0)
hideturtle()

def box(x=None,y=None,size=100,angle=None):
    penup()
    
    if x is not None or y is not None:
        goto(x,y)

    if angle is not None:
        setheading(angle)

    pendown()

    for i in range(4):
        forward(size)
        left(90)

    penup()



def sweep(x,y,size,start,end,step):
    for angle in range(start, end, step):
        box(x,y,size,angle)


def diamond(x,y,size,step=3):
    sweep(x,y,size,30,60,step)


#diamond(100,100,200,step=1)


# 4 different ways to call box
box()
box(100,100)
box(100,100,200)
box(100,100,200,45)

# more ways to call box with keywords
box(size=300)
box(size=300,angle=30)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
