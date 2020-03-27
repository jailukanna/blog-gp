---
title: turtle example projects treeProject shapes (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'projects treeProject shapes'

Functions in program: 
* `def draw_star(turtle, color, x, y, size):`
* `def draw_rectangle(turtle, color, x, y, width, height):`
* `def draw_square(turtle, color, x, y, size):`
* `def draw_triangle(turtle, color, x, y, size):`
* `def draw_circle(turtle, color, x, y, radius):`

Modules used in program: 
* `import turtle`

## python projects treeProject shapes

Python turtle example: projects treeProject shapes

```python
# This is a custom module we've mapes.
# Modules are files full of code that you can import into your programs.
# This one teaches our turtle to draw various sh
import turtle

def draw_circle(turtle, color, x, y, radius):
  turtle.penup()
  turtle.color(color)
  turtle.fillcolor(color)
  turtle.goto(x,y)
  turtle.pendown()
  turtle.begin_fill()
  turtle.circle(radius)
  turtle.end_fill()
    
def draw_triangle(turtle, color, x, y, size):
  turtle.penup()
  turtle.color(color)
  turtle.fillcolor(color)
  turtle.goto(x,y)
  turtle.pendown()
  turtle.begin_fill()
  for i in range (3):
    turtle.forward(size)
    turtle.left(120)
  turtle.end_fill()
  turtle.setheading(0)
    
def draw_square(turtle, color, x, y, size):
  turtle.penup()
  turtle.color(color)
  turtle.fillcolor(color)
  turtle.goto(x,y)
  turtle.pendown()
  turtle.begin_fill()
  for i in range (4):
    turtle.forward(size)
    turtle.left(90)
  turtle.end_fill()
  turtle.setheading(0)
  
def draw_rectangle(turtle, color, x, y, width, height):
  turtle.penup()
  turtle.color(color)
  turtle.fillcolor(color)
  turtle.goto(x,y)
  turtle.pendown()
  turtle.begin_fill()
  for i in range (2):
    turtle.forward(width)
    turtle.left(90)
    turtle.forward(height)
    turtle.left(90)
    
  turtle.end_fill()
  turtle.setheading(0)  
  
def draw_star(turtle, color, x, y, size):
  turtle.penup()
  turtle.color(color)
  turtle.fillcolor(color)
  turtle.goto(x,y)
  turtle.pendown()
  turtle.begin_fill()
  turtle.right(144)
  for i in range(5):
    turtle.forward(size)
    turtle.right(144)
    turtle.forward(size)
  turtle.end_fill()
  turtle.setheading(0)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
