---
title: turtle example Rangoli (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Rangoli'

Functions in program: 
* `def Gol(r,c1,c2):`

## python Rangoli

Python turtle example: Rangoli

```python
from turtle import *
bob = Turtle()
def Gol(r,c1,c2):
	penup()
	right(90)
	fd(r)
	left(90)
	pendown()
	color(c1,c2)
	begin_fill()
	circle(r)
	end_fill()
	penup()
	left(90)
	fd(r)
	right(90)
	pendown()	
# C1
Gol(170,'black','#f7aaa1')
left(30)
for i in range(5):
	penup()
	left(108)
	fd(190)
	pendown()
	penup()
	right(90)
	fd(15)
	left(90)
	pendown()
	color('black','#68f909')
	begin_fill()
	circle(15)
	end_fill()
	penup()
	left(90)
	fd(15)
	right(90)
	penup()
	# circle
	left(180)
	fd(190)
right(30)
pendown()
for i in range(6):
	color('black','red')
	begin_fill()
	heading = bob.heading()
	circle(200, 60)
	left(120)
	circle(200, 60)
	setheading(heading)
	bob.left(360 / 5)
	end_fill()
Gol(120,'black','#fced05')
Gol(100,'white','#5fa7b5')
for i in range(11):
	color('white','#ff9933')
	begin_fill()
	heading = bob.heading()
	circle(100, 60)
	left(120)
	circle(100, 60)
	setheading(heading)
	bob.left(360 / 10)
	end_fill()
Gol(40,'white','#fced05')
for i in range(10):
	color('white','red')
	begin_fill()
	heading = bob.heading()
	circle(60, 60)
	left(120)
	circle(60, 60)
	setheading(heading)
	bob.left(360 / 10)
	end_fill()
Gol(20,'white','white')
bob.hideturtle()
screen = Screen()
screen.exitonclick()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
