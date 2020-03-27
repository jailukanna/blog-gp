---
title: turtle example golden rectangle (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'golden rectangle'

Functions in program: 
* `def golden_rectangle(side_length, num_of_rectangles):`
* `def draw_square(length):`

Modules used in program: 
* `import math`

## python golden rectangle

Python turtle example: golden rectangle

```python
import math
from turtle import *
godlen_ratio = (1+math.sqrt(5))/2 # calculated golden ratio

#Instantiate attributes of the Turtle
turtle = Turtle()
turtle.speed(0) # Adjust speed of drawing
turtle.color('red')
turtle.pencolor("blue")

def draw_square(length):
  # draw 4 sides
  for i in range(4):
    turtle.forward(length)
    turtle.right(90)


turtle.penup()
turtle.goto(-200,-100)
turtle.setheading(90)
turtle.pendown()



def golden_rectangle(side_length, num_of_rectangles):

    for i in range(num_of_rectangles):
      draw_square(side_length)
      turtle.forward(side_length)
      turtle.right(90)
      turtle.forward(side_length)
      side_length = side_length/godlen_ratio




side_length = 200 # Change to affect rectangle sizes
number_of_rectangles = 10 # Change to affect how many rectangles are made
golden_rectangle(side_length, number_of_rectangles)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
