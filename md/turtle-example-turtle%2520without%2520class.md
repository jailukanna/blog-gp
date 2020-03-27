---
title: turtle example turtle%2520without%2520class (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtle%2520without%2520class'


Modules used in program: 
* `import turtle`

## python turtle%2520without%2520class

Python turtle example: turtle%2520without%2520class

```python
import turtle
from turtle import *

screen = turtle.Screen()
turtle.setup(1024, 768)
screen.bgcolor("#FFFFE0")

t1 = turtle.Turtle()
t1.penup()
t1.goto(-500,100)
t1.pendown()
t1.shape("circle")
t1.color("red")
t1.pensize(4)
t1.forward(500)

t2 = turtle.Turtle()
t2.penup()
t2.goto(-500,50)
t2.pendown()
t2.shape("square")
t2.color("pink")
t2.pensize(7)
t2.forward(600)

t3 = turtle.Turtle()
t3.penup()
t3.goto(-500,0)
t3.pendown()
t3.shape("triangle")
t3.color("blue")
t3.pensize(9)
t3.forward(400)

t4 = turtle.Turtle()
t4.penup()
t4.goto(-500,-50)
t4.pendown()
t4.shape("turtle")
t4.color("brown")
t4.pensize(1)
t4.forward(200)

exitonclick()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
