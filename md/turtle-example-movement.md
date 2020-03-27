---
title: turtle example movement (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'movement'

Functions in program: 
* `def move_turtle_down():`
* `def move_turtle_up():`
* `def move_turtle_right():`
* `def move_turtle_left():`

## python movement

Python turtle example: movement

```python
from turtle import Turtle, Screen
screen = Screen()
screen.setup(400, 400)
screen.screensize(380, 380)
screen.tracer(0)
t = Turtle()
t_right = 0.0
t.up = 90.0
t.down = 270.0
t.lef = 180.0


def move_turtle_left():
    if t.heading() == 90.0:
        t.left(90)
    if t.heading() == 270.0:
        t.right(90)
    t.forward(10)


def move_turtle_right():
    if t.heading() == 90.0:
        t.right(90)
    if t.heading() == 270.0:
        t.left(90)

    t.forward(10)


def move_turtle_up():
    if t.heading() != 90.0:
        t.left(90)
    if t.heading() == 180.0:
        t.right(90)

    t.forward(10)


def move_turtle_down():
    if t.heading() == 180.0:
        t.left(90)
    if t.heading() == 0.0:
        t.right(90)
    t.forward(10)


screen.listen()
screen.onkeypress(move_turtle_left, 'Left')
screen.onkeypress(move_turtle_right, 'Right')
screen.onkeypress(move_turtle_up, 'Up')
screen.onkeypress(move_turtle_down, 'Down')

while True:
    screen.update()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
