---
title: turtle example TurtleGame (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'TurtleGame'


## python TurtleGame

Python turtle example: TurtleGame

```python
# start Turtle Here

from turtle import *
from random import randint
speed(0)
penup()
pensize(2)
pencolor('lightblue')
fillcolor("blue")
hideturtle()
goto(-150,150)
for step in range(15):
  write(step, align='center')
  right(90)
  for num in range(8):
    penup()
    forward(10)
    pendown()
    forward(10)
  penup()
  backward(160)
  left(90)
  forward(20)
hideturtle()

#Ist turtle ada

Rahul = Turtle()
Rahul.color("red")
Rahul.shape("turtle")
Rahul.penup()
Rahul.goto(-165,120)
Rahul.right(360)
Rahul.pendown()
Rahul.write("Rahul   ",align ="right")

# 2nd turtle bob

ajaz = Turtle()
ajaz.color("turquoise")
ajaz.shape("turtle")
ajaz.penup()
ajaz.goto(-165,90)
ajaz.right(360)
ajaz.pendown()
ajaz.write("ajaz     ",align ="right")

# 3nd turtle bob

apoorv = Turtle()
apoorv.color("violet")
apoorv.shape("turtle")
apoorv.penup()
apoorv.goto(-165,60)
apoorv.right(360)
apoorv.pendown()
apoorv.write("apoorv  ",align ="right")

# 4th turtle bob

shams = Turtle()
shams.color("lime")
shams.shape("turtle")
shams.penup()
shams.goto(-165,30)
shams.right(360)
shams.pendown()
shams.write("shams  ",align ="right")

for turn in range(100):
    Rahul.forward(randint(1,5))
    ajaz.forward(randint(1,5))
    apoorv.forward(randint(1,5))
    shams.forward(randint(1,5))
  

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
