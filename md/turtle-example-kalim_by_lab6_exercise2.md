---
title: turtle example kalim by lab6 exercise2 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'kalim by lab6 exercise2'

Functions in program: 
* `def display_histogram_turtle():`
* `def display_histogram_ascii():`
* `def display_count_list():`
* `def display_list():`
* `def create_list():`
* `def histogram():`
* `def bar(x_position, height):`
* `def axis_y():`
* `def axis_x():`

## python kalim by lab6 exercise2

Python turtle example: kalim by lab6 exercise2

```python
# Name:     Kalim BY
# Class:    Python Programming class

from random import randint, random
from turtle import *

TOTAL = 100
MAX = 20
START = 0
END = 9

BAR_H_SCALE = 20

# HISTOGRAPH
turtle = Turtle(visible=False)


def axis_x():
    """
    Draw X Ruler
    """
    turtle.up()
    turtle.goto(-400, -125)
    turtle.down()
    turtle.fd(800)
    turtle.lt(160)
    turtle.fillcolor("black")
    turtle.begin_fill()
    turtle.fd(20)
    turtle.lt(110)
    turtle.fd(15)
    turtle.lt(110)
    turtle.fd(20)
    turtle.end_fill()
    x = -365
    y = -125
    for i in range(END + 1):
        turtle.up()
        turtle.goto(x, y - 20)
        turtle.down()
        turtle.write(chr(48 + i))
        x += 70
    pass


def axis_y():
    """
    Draw Y Ruler
    """
    turtle.up()
    turtle.goto(-400, -125)
    turtle.down()
    turtle.lt(70)
    turtle.fd(( BAR_H_SCALE * 10) + 50)
    turtle.lt(160)
    turtle.fillcolor("black")
    turtle.begin_fill()
    turtle.fd(20)
    turtle.lt(110)
    turtle.fd(15)
    turtle.lt(110)
    turtle.fd(20)
    turtle.end_fill()
    turtle.rt(20)
    x = -400
    y = -125
    for i in range(10):
        turtle.up()
        y += BAR_H_SCALE
        turtle.goto(x - 5, y)
        turtle.down()
        turtle.goto(x + 5, y)
        turtle.up()
        turtle.goto(x - 25, y - 7)
        turtle.write((i + 1) * 10)
    pass


def bar(x_position, height):
    """
    Draw Bar
    :param x_position:
    :param height:
    :return:
    """
    turtle.up()
    turtle.goto(-400 + x_position, -125)
    turtle.down()
    turtle.fillcolor((random(), random(), random()))  # rgb
    turtle.begin_fill()
    for i in range(2):
        turtle.fd(height)
        turtle.rt(90)
        turtle.fd(70)
        turtle.rt(90)
    turtle.end_fill()


def histogram():
    turtle.screen.setup(1000, 350)
    turtle.screen.title('Histogram')
    turtle.ht()
    x = 0
    axis_x()
    axis_y()
    for i in range(END + 1):
        y = nums.count(i) * BAR_H_SCALE / 10
        bar(x, y)
        x += 70


# EXERCISE
def create_list():
    """
    Generate 100 numbers randomly and assign them to a list of numbers
    """
    for i in range(int(TOTAL / MAX)):
        for j in range(MAX):
            num = randint(START, END)
            nums.append(num)


def display_list():
    """
    Display the numbers in the list with 20 on each line
    """
    print()
    print("Display: ", end="")
    counter = 0
    for i in nums:
        if (counter % MAX) == 0:
            print()
        print(i, end=" ")
        counter += 1
    print()
    pass


def display_count_list():
    """
    Display the counts list
    """
    print()
    print("Count List")
    for i in range(START, END + 1):
        print("Number", i, ":", nums.count(i))
    pass


def display_histogram_ascii():
    """"
    Display the results in the form of histogram
    """
    print()
    print("Histogram")
    for i in range(START, END + 1):
        print(i, end=": ")
        for j in range(nums.count(i)):
            print("+", end="")
        print()
    pass


def display_histogram_turtle():
    histogram()
    turtle.screen.mainloop()
    pass


if __name__ == '__main__':
    nums = []
    create_list()
    display_list()
    display_count_list()
    display_histogram_ascii()
    display_histogram_turtle()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
