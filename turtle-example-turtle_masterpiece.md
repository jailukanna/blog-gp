---
title: turtle example turtle masterpiece (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtle masterpiece'

Functions in program: 
* `def draw_art():`
* `def draw_square(some_turtle):`

## python turtle masterpiece

Python turtle example: turtle masterpiece

```python
from turtle import Turtle, Screen

def draw_square(some_turtle):

    for _ in range(4):
        some_turtle.forward(200)
        some_turtle.right(90)

def draw_art():

    # Turtle Brad - Square Circle
    Brad = Turtle(shape="turtle")
    Brad.color("#f9f262")
    Brad.pensize(2)
    Brad.speed("fastest")  

    for _ in range(36):
        draw_square(Brad)
        Brad.right(10)
        
    Brad.hideturtle()

    # Turtle Angie - Swirl
    Angie = Turtle(shape="turtle")
    Angie.color("blue")
    Angie.pensize(2)
    Angie.speed("fastest")  

    size = 1
    for _ in range(285):
        Angie.forward(size)
        Angie.right(91)
        size += 1
        
    Angie.hideturtle()

    # Turtle Tony - Medium Sacred G
    Tony = Turtle(shape ="turtle")
    Tony.color("#62c0f9")
    Tony.pensize(2)
    Tony.speed("fast")
    Tony.circle(100)
    Tony.setheading(60)
    Tony.circle(100)
    Tony.setheading(120)
    Tony.circle(100)
    Tony.setheading(180)
    Tony.circle(100)
    Tony.setheading(240)
    Tony.circle(100)
    Tony.setheading(300)
    Tony.circle(100)
    Tony.hideturtle()

    # Turtle Thomas - Small Sacred G
    Thomas = Turtle(shape ="turtle")
    Thomas.color("white")
    Thomas.pensize(2)
    Thomas.speed("fast")
    Thomas.circle(50)
    Thomas.setheading(60)
    Thomas.circle(50)
    Thomas.setheading(120)
    Thomas.circle(50)
    Thomas.setheading(180)
    Thomas.circle(50)
    Thomas.setheading(240)
    Thomas.circle(50)
    Thomas.setheading(300)
    Thomas.circle(50)
    Thomas.hideturtle()
    
    # Turtle Sally - Circle
    Sally = Turtle(shape = "turtle")
    Sally.color("#ee0a0a")
    Sally.pensize(15)
    Sally.speed("fastest")
    Sally.penup()
    Sally.setposition(0,-280)
    Sally.pendown()
    Sally.circle(280)
    Sally.hideturtle()
 
window= Screen()
window.bgcolor("black")

draw_art()

window.exitonclick()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
