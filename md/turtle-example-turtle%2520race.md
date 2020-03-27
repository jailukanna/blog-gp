---
title: turtle example turtle%2520race (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtle%2520race'


Modules used in program: 
* `import time`
* `import turtle`

## python turtle%2520race

Python turtle example: turtle%2520race

```python
import turtle
import time
from turtle import Turtle
from random import randint


#set up the screen
ms = turtle.Screen()
ms.title("Tortoise Speed Racing..  by @Rafa")
ms.bgcolor("green")
turtle.speed(0)
turtle.penup()
turtle.setpos(-140, 200)
turtle.write("SPEED COURSE", font=( "Arial", 30, "bold"))
turtle.penup()

# dirt
turtle.setpos(-400, -180)
turtle.color("Chocolate")
turtle.begin_fill()
turtle.pendown()
turtle.forward(800)
turtle.right(90)
turtle.forward(300)
turtle.right(90)
turtle.forward(800)
turtle.right(90)
turtle.forward(300)
turtle.end_fill()

# finish line!
stamp_size = 20
square_size = 15
finish_line = 200

turtle.color("black")
turtle.shape("square")
turtle.shapesize(square_size / stamp_size)
turtle.penup()

for i in range(10):
    turtle.setpos(finish_line, (150 -(i * square_size * 2)))
    turtle.stamp()

for i in range(10):
    turtle.setpos(finish_line + square_size, ((150 - square_size) - (i * square_size * 2)))
    turtle.stamp()

turtle.hideturtle()




#player
nips = turtle.Turtle()
nips.speed(0)
nips.color("white")
nips.shape("turtle")
nips.penup()
nips.goto(-250, 100)
nips.pendown()

#player2
chips = turtle.Turtle()
chips.speed(0)
chips.color("red")
chips.shape("turtle")
chips.penup()
chips.goto(-250, 50)
chips.pendown()

#player3
hips = turtle.Turtle()
hips.speed(0)
hips.color("blue")
hips.shape("turtle")
hips.penup()
hips.goto(-250, 0)
hips.pendown()


#player4
carl = turtle.Turtle()
carl.speed(0)
carl.color("silver")
carl.shape("turtle")
carl.penup()
carl.goto(-250, -50)
carl.pendown()


#player5
ryan = turtle.Turtle()
ryan.speed(0)
ryan.color("yellow")
ryan.shape("turtle")
ryan.penup()
ryan.goto(-250, 150)
ryan.pendown()

#player6
yan = turtle.Turtle()
yan.speed(0)
yan.color("purple")
yan.shape("turtle")
yan.penup()
yan.goto(-250, -110)
yan.pendown()



time.sleep(1) # Pause's the game for 1 second before starting the race!
# MOVE THE PLAYERS
for i in range(145):
    nips.forward(randint(1,5))
    chips.forward(randint(1, 5))
    hips.forward(randint(1, 5))
    carl.forward(randint(1, 5))
    ryan.forward(randint(1, 5))
    yan.forward(randint(1, 5))

turtle.exitonclick(

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
