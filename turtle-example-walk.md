---
title: turtle example walk (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'walk'

Functions in program: 
* `def moveto(x,y):`

Modules used in program: 
* `import turtle`

## python walk

Python turtle example: walk

```python
import turtle
from random import randint

turtle.hideturtle()
turtle.speed('fastest')

def moveto(x,y):
    turtle.penup()
    turtle.goto(x,y)
    turtle.pendown()

turtle.onscreenclick(moveto)

while True:    
    r = randint(0, 360)
    turtle.setheading(r)
    turtle.forward(10)          


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
