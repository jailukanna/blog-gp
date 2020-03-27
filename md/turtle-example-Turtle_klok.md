---
title: turtle example Turtle klok (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Turtle klok'


## python Turtle klok

Python turtle example: Turtle klok

```python
# Sine klokje...zoals het nergens tikt!

from datetime import datetime
from turtle import *

## canvas
screen = Screen()
screen.tracer(0, 25)    # het magische script blokje -- tracer --!!!

## turtle voor het tekenen van het klokje
myPen = Turtle()
myPen.shape("arrow")
myPen.hideturtle()
myPen.speed(0)

## turtle voor de urenwijzer
hour = Turtle()
hour.shape("classic")
hour.pensize(2)
hour.color("blue")

## turtle voor de minutenwijzer
minute = Turtle()
minute.shape("classic")
minute.pensize(2)
minute.color("green")

## turtle voor de secondenwijzer
second = Turtle()
second.hideturtle()
second.color("red")

## variabele voor uren minuten en seconden
currentHour = datetime.now().hour
currentMinute = datetime.now().minute
currentSecond = datetime.now().second

# tekenen eerst het frame van de klok
myPen.penup()
myPen.goto(0, 200)
myPen.write("SINE KLOKJE", font=("Georgia",21), align="center")

myPen.penup()
myPen.goto(0, -180)
myPen.pendown()
myPen.color("black")
myPen.circle(180)

for i in range(1,13):
    myPen.penup()
    myPen.goto(0,0)
    myPen.setheading(90) # richt de turtle naar boven - 12 uur
    myPen.right(i*360/12)
    myPen.forward(155)
    myPen.pendown()
    myPen.forward(25)

for i in range(1,61):
    myPen.penup()
    myPen.goto(0,0)
    myPen.setheading(90) # richt de turtle naar boven - 12 uur
    myPen.right(i*360/60)
    myPen.forward(170) 
    myPen.pendown()
    myPen.forward(10)


while True:
    hour.clear()
    minute.clear()
    second.clear()
    
    currentHour = datetime.now().hour
    currentMinute = datetime.now().minute
    currentSecond = datetime.now().second

    #uurwijzer
    hour.penup()
    hour.goto(0,0)
    hour.setheading(90)
    hour.right(0.5 * (60 * currentHour + currentMinute))    # formule via wikipedia: https://en.m.wikipedia.org/wiki/Clock_angle_problem#Equation_for_the_angle_of_the_hour_hand
    hour.pendown()
    hour.forward(100)
    
    #minutenwijzer
    minute.penup()
    minute.goto(0,0)
    minute.setheading(90)
    minute.right(currentMinute * 360/60)
    minute.pendown()
    minute.forward(150)

    #secondenwijzer
    second.penup()
    second.goto(0,0)
    second.setheading(90)
    second.right(currentSecond * 360/60)
    second.pendown()
    second.forward(150)
    screen.update()     # ververs het scherm
    



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
