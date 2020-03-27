---
title: turtle example Python%2520Homework%252006.12.2019 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Python%2520Homework%252006.12.2019'

Functions in program: 
* `def display_histogram():`
* `def display_count_list():`
* `def display_20_list():`
* `def create_list():`
* `def histogram():`
* `def bar(x_position, height):`
* `def axis_y():`
* `def axis_x():`

## python Python%2520Homework%252006.12.2019

Python turtle example: Python%2520Homework%252006.12.2019

```python
from random import randint, random
from turtle import *

TOTAL = 100
MAX = 20
START = 0
END = 9
BAR_H_SCALE = 25
turtle = Turtle(visible=False)

def axis_x():
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
    turtle.up()
    turtle.goto(-400 + x_position, -125)
    turtle.down()
    turtle.fillcolor((random(), random(), random()))
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
        y = numbers.count(i) * BAR_H_SCALE / 10
        bar(x, y)
        x += 70

def create_list():
    for i in range(int(TOTAL / MAX)):
        for j in range(MAX):
            num = randint(START, END)
            numbers.append(num)


def display_20_list():
    print("Display List: ")
    counter = 0
    for i in numbers:
        if (counter % MAX) == 0:
            print()
        print(i, end=" ")
        counter += 1
    print()
    print()
    print()
    pass


def display_count_list():
    print("Count List: \n")
    for i in range(START, END + 1):
        print("Number", i, ":", numbers.count(i))
    pass
    print()


def display_histogram():
    histogram()
    turtle.screen.mainloop()
    pass


if __name__ == '__main__':
    print("Student: Furkan KURT\n")
    numbers = []
    create_list()
    display_20_list()
    display_count_list()
    display_histogram()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
