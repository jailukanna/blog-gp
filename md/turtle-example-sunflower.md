---
title: turtle example sunflower (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'sunflower'

Functions in program: 
* `def main():`
* `def draw():`
* `def prepare():`
* `def leaf(direction, size):`
* `def branch(direction, size):`
* `def flower(size):`
* `def rhombus(direction, size):`

## python sunflower

Python turtle example: sunflower

```python
from time import sleep
from turtle import *

def rhombus(direction, size):
    setheading(direction)

    for angle in [35, 145, 35, 145]:
        forward(size)
        left(angle)

def flower(size):
    fillcolor('yellow')
    begin_fill()

    for angle in range(0, 360, 15):
        rhombus(angle, size)

    end_fill()

def branch(direction, size):
    setheading(direction)
    forward(size)

def leaf(direction, size):
    fillcolor('green')
    begin_fill()
    rhombus(direction, size)
    end_fill()

def prepare():
    penup()
    goto(0, 100)
    speed(10)
    pencolor('black')
    pendown()

def draw():
    flower(100)
    branch(270, 400)
    leaf(15, 150)
    leaf(130, 150)

def main():
    title('Sunflower')
    prepare()
    sleep(2)
    draw()
    exitonclick()

if __name__ == "__main__":
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
