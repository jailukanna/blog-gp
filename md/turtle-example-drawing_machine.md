---
title: turtle example drawing machine (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'drawing machine'

Functions in program: 
* `def draw_numbers(state_sequence):`
* `def draw_number(number):`
* `def draw_title(number):`
* `def move_right():`
* `def draw(state):`
* `def get_number(numbers):`

## python drawing machine

Python turtle example: drawing machine

```python
"""
Utility to facilitate the visualization of tasks https://code.google.com/codejam/contest/3214486/dashboard#s=p0

Requirements for Arch Linux: 
Python >= 3.5
$ sudo pacman -S tk

Using:
from drawing_machine import draw_numbers
draw_numbers('1111111 1101011')
# Click on the canvas to close the program
"""

from turtle import *

# setup
pensize(5)
speed(0)
penup()
goto(-350, 300)
pendown()


def get_number(numbers):
    for i in numbers:
        yield int(i)


def draw(state):
    if state:
        pensize(7)
        color("red")
    else:
        pensize(2)
        color("gray")


def move_right():
    penup()
    forward(120)
    pendown()


def draw_title(number):
    current_position = position()
    penup()
    left(90)
    forward(20)
    right(90)
    forward(10)
    color("black")
    write(number, align="left", font=("Arial", 15, "normal"))
    goto(current_position)
    pendown()


def draw_number(number):
    draw_title(number)
    states = get_number(number)
    draw(next(states))
    forward(100)
    right(90)
    draw(next(states))
    forward(100)
    draw(next(states))
    forward(100)
    right(90)
    draw(next(states))
    forward(100)
    right(90)
    draw(next(states))
    forward(100)
    right(90)
    draw(next(states))
    forward(100)
    backward(100)
    left(90)
    draw(next(states))
    forward(100)
    right(90)
    move_right()


def draw_numbers(state_sequence):
    """
    Draws a sequence of numbers
    :param str state_sequence: '1011011 1011111'
    """
    numbers = state_sequence.split()
    for number in numbers:
        draw_number(number)
    exitonclick()


if __name__ == "__main__":
    draw_numbers("1011011 1011111 1010000 1011111 1011011")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
