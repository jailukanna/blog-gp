---
title: turtle example name-initials (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'name-initials'

Functions in program: 
* `def letter_I(turtle):`
* `def letter_H(turtle):`
* `def letter_B(turtle):`
* `def letter_A(turtle):`

## python name-initials

Python turtle example: name-initials

```python

from turtle import Turtle, Screen

NAME = "HABIB"

BORDER = 1
BOX_WIDTH, BOX_HEIGHT = 6, 10  # boundray for letters
WIDTH, HEIGHT = BOX_WIDTH - BORDER * 2, BOX_HEIGHT - BORDER * 2  # size for letters

def letter_A(turtle):
    turtle.forward(HEIGHT / 2)
    for _ in range(3):
        turtle.forward(HEIGHT / 2)
        turtle.right(90)
        turtle.forward(WIDTH)
        turtle.right(90)
    turtle.forward(HEIGHT)

def letter_B(turtle):
    turtle.forward(HEIGHT)
    turtle.right(90)
    turtle.circle(-HEIGHT / 4, 180, 40)
    turtle.right(180)
    turtle.circle(-HEIGHT / 4, 180, 40)

def letter_H(turtle):
    turtle.forward(HEIGHT)
    turtle.right(180)
    turtle.forward(HEIGHT/2)
    turtle.left(90)
    turtle.forward(HEIGHT/2)
    turtle.left(90)
    turtle.forward(HEIGHT/2)
    turtle.right(180)
    turtle.forward(HEIGHT)
    turtle.right(90)
   
def letter_I(turtle):
    turtle.right(90)
    turtle.forward(WIDTH)
    turtle.backward(WIDTH / 2)
    turtle.left(90)
    turtle.forward(HEIGHT)
    turtle.right(90)
    turtle.backward(WIDTH / 2)
    turtle.forward(WIDTH)


LETTERS = {'A': letter_A, 'B':letter_B, 'H': letter_H, 'I': letter_I}

sc = Screen()
sc.setup(800, 400)  # screen size
sc.title("Name Initials: {}".format(NAME)) #screen Title
sc.setworldcoordinates(0, 0, BOX_WIDTH * len(NAME), BOX_HEIGHT)

marker = Turtle()

for counter, letter in enumerate(NAME):
    marker.penup()
    marker.goto(counter * BOX_WIDTH + BORDER, BORDER)
    marker.setheading(90)

    if letter in LETTERS:
        marker.pendown()
        LETTERS[letter](marker)

marker.hideturtle()

sc.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
