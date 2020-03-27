---
title: turtle example logo pythonizame turtle (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'logo pythonizame turtle'

Functions in program: 
* `def dibuja_python(velocidad):`
* `def dibuja_rombo(velocidad):`

Modules used in program: 
* `import turtle`

## python logo pythonizame turtle

Python turtle example: logo pythonizame turtle

```python
# coding=utf-8
__author__ = 'gaspar'

from turtle import *
import turtle


setup(1024,900) #configura el tamaño de la ventana

def dibuja_rombo(velocidad):
    turtle.speed(velocidad)
    turtle.pensize(5)
    turtle.penup()
    turtle.right(90)
    turtle.forward(100)
    turtle.right(90)
    turtle.forward(25)
    turtle.pencolor("blue")
    turtle.right(180)
    turtle.pendown()
    turtle.left(40)

    for i in range(3):
        turtle.forward(200)
        turtle.left(90)


    turtle.forward(200)
    turtle.left(135)
    turtle.forward(277)
    turtle.penup()
    turtle.backward(300)
    turtle.write("Pythonizame")
    turtle.hideturtle()

def dibuja_python(velocidad):
    pl = turtle.Turtle()
    pl.speed(velocidad)
    pl.pensize(5)
    pl.fillcolor("gray")
    pl.penup()
    pl.left(90)
    pl.forward(110)
    pl.right(90)
    pl.forward(20)
    pl.pendown()
    pl.begin_fill()
    pl.backward(75)
    pl.right(90)
    pl.forward(50)
    pl.left(90)
    pl.forward(50)
    pl.backward(100)
    pl.right(90)
    pl.forward(50)
    pl.left(90)
    pl.forward(50)
    pl.left(90)
    pl.forward(25)
    pl.right(90)
    pl.forward(75)
    pl.left(90)
    pl.forward(75)
    pl.end_fill()

    # segundo python
    pl.fillcolor("gray")
    pl.begin_fill()
    pl.backward(50)
    pl.right(90)
    pl.forward(50)
    pl.right(90)
    pl.forward(50)
    pl.right(90)
    pl.forward(100)
    pl.backward(50)
    pl.left(90)
    pl.forward(50)
    pl.right(90)
    pl.forward(75)
    pl.right(90)
    pl.forward(75)
    pl.right(90)
    pl.forward(75)
    pl.left(90)
    pl.forward(50)
    pl.end_fill()
    pl.left(90)
    pl.penup()
    pl.forward(50)
    pl.pendown()
    pl.dot(10,"white")
    pl.penup()
    pl.left(90)
    pl.forward(100)
    pl.left(90)
    pl.forward(30)
    pl.pendown()
    pl.dot(10,"white")
    pl.hideturtle()


dibuja_rombo(5)
dibuja_python(5)

turtle.exitonclick() #permite que la ventana se cierre al hacer clic dentro

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
