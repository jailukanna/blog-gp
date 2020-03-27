---
title: turtle example turtlecon (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtlecon'


Modules used in program: 
* `import turtle`

## python turtlecon

Python turtle example: turtlecon

```python
“Turtle” is a Python feature like a drawing board, which lets us command a turtle to draw all over it! We can use functions like turtle.forward(…) and turtle.right(…) which can move the turtle around.Commonly used turtle methods are :

METHOD	PARAMETER	DESCRIPTION
Turtle()	None	Creates and returns a new tutrle object
forward()	amount	Moves the turtle forward by the specified amount
backward()	amount	Moves the turtle backward by the specified amount
right()	angle	Turns the turtle clockwise
left()	angle	Turns the turtle counter clockwise
penup()	None	Picks up the turtle’s Pen
pendown()	None	Puts down the turtle’s Pen
up()	None	Picks up the turtle’s Pen
down()	None	Puts down the turtle’s Pen
color()	Color name	Changes the color of the turtle’s pen
fillcolor()	Color name	Changes the color of the turtle will use to fill a polygon
heading()	None	Returns the current heading
position()	None	Returns the current position
goto()	x, y	Move the turtle to position x,y
begin_fill()	None	Remember the starting point for a filled polygon
end_fill()	None	Close the polygon and fill with the current fill color
dot()	None	Leave the dot at the current position
stamp()	None	Leaves an impression of a turtle shape at the current location
shape()	shapename	Should be ‘arrow’, ‘classic’, ‘turtle’ or ‘circle’


importing turtle
from turtle import *
# or
import turtle



making a board
wn = turtle.Screen()
wn.bgcolor("light green")
wn.title("Turtle")
skk = turtle.Turtle()


forward movement
skk.forward(100)

turtle done
turtle.done()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
