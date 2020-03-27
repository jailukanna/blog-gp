---
title: turtle example turtle%2520with%2520class (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtle%2520with%2520class'


Modules used in program: 
* `import turtle`

## python turtle%2520with%2520class

Python turtle example: turtle%2520with%2520class

```python
import turtle
from turtle import *

screen = turtle.Screen()
turtle.setup(1024, 768)
screen.bgcolor("#FFFFE0")

class Line(turtle.Turtle):
    def __init__(self, color, shape, pensize, length, points):
        turtle.Turtle.__init__(self)
        self.penup()
        self.goto(points)
        self.pendown()
        self.color(color)
        self.shape(shape)
        self.pensize(pensize)
        self.forward(length)

t1 = Line("red", "circle", 4, 500, (-500,100))
t2 = Line("pink", "square", 7, 400,(-500,50))
t3 = Line("blue", "triangle", 9, 600, (-500,0))
t4 = Line("brown", "turtle", 1, 200,(-500,-50))

exitonclick()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
