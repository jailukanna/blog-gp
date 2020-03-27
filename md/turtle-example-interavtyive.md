---
title: turtle example interavtyive (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'interavtyive'

Functions in program: 
* `def main():`
* `def size_down():`
* `def size_up():`
* `def change_color():`
* `def draw_down():`
* `def draw_up():`
* `def draw_bkw():`
* `def draw_fwd():`
* `def draw_circle():`

## python interavtyive

Python turtle example: interavtyive

```python
from turtle import *
from random import randint

pensize(1)
color('red')

p_size = 1
my_colors = ['red', 'green', 'blue', 'yellow', 'purple']

def draw_circle():
    circle(40)

def draw_fwd():
    setheading(0)
    forward(10)

def draw_bkw():
    setheading(180)
    forward(10)

def draw_up():
    setheading(90)
    forward(10)

def draw_down():
    setheading(270)
    forward(10)

def change_color():
    i = randint(0, len(my_colors)-1)
    color(my_colors[i])


def size_up():
    p_size += 1
    pensize(p_size)


def size_down():
    p_size -= 1
    pensize(p_size)

def main():
    onkeypress(draw_fwd, 'Right')
    onkeypress(draw_bkw, 'Left')
    onkeypress(draw_up, 'Up')
    onkeypress(draw_down, 'Down')
    onkeypress(change_color, 'c')
    listen()
    mainloop()

if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
