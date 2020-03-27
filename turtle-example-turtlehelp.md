---
title: turtle example turtlehelp (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtlehelp'

Functions in program: 
* `def comienzo(tortuga):`
* `def estrella(tortuga, a, b):`
* `def poligono(tortuga, k):`
* `def escalera(tortuga):`
* `def espiral(tortuga):`

## python turtlehelp

Python turtle example: turtlehelp

```python
from turtle import *
from types import MethodType

def espiral(tortuga):
    for i in range(100):
        tortuga.forward(i)
        tortuga.left(15)

def escalera(tortuga):
    for i in range(20):
        tortuga.forward(10)
        if i%2 == 0:
            tortuga.left(90)
        else:
            tortuga.right(90)

def poligono(tortuga, k):
    for i in range(k):
        tortuga.forward(100)
        tortuga.left(360/k)

def estrella(tortuga, a, b):
    tortuga.begin_fill()
    for i in range(a):
        tortuga.forward(100)
        tortuga.left(360*b/a)
    tortuga.end_fill()

def comienzo(tortuga):
    tortuga.penup()
    tortuga.goto(0,0)
    tortuga.seth(0)
    tortuga.pendown()

Turtle.espiral = espiral
Turtle.escalera = escalera
Turtle.poligono = poligono
Turtle.estrella = estrella
Turtle.comienzo = comienzo

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
