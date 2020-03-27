---
title: turtle example drawUtils (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'drawUtils'

Functions in program: 
* `def drawHouse(size, x = 0, y = 0):`
* `def drawTriangle(h, w, x = None, y = None, useFill = True, usePen = True, penColor = "Black", fillColor = "Black", penSize = 1):`
* `def drawCircle(size,`
* `def drawBox(x1, x2, y1, y2, penColor="Black", fillColor="Black"):`

Modules used in program: 
* `import time`

## python drawUtils

Python turtle example: drawUtils

```python
#import sys #Currently not needed
import time
from turtle import *

#Draw a box
def drawBox(x1, x2, y1, y2, penColor="Black", fillColor="Black"):
    turtle = Turtle()
    d = delay()
    delay(0)
    turtle.hideturtle()
    turtle.up()
    turtle.pencolor(penColor)
    turtle.fillcolor(fillColor)
    turtle.goto(x1, y1)
    turtle.down()
    turtle.begin_fill()
    turtle.goto(x1, y2)
    turtle.goto(x2, y2)
    turtle.goto(x2, y1)
    turtle.goto(x1, y1)
    turtle.end_fill()
    turtle.up()
    del turtle
    delay(d)
    del d
    
def drawCircle(size,
               x = None, y = None,
               usePen = True,
               useFill = True,
               fillColor = "Black",
               penColor = ["Black"],
               penSize = 5,
               pieces = 1):
    if (pieces < 1):
        pieces = 1
    if not(usePen) and not(useFill):
        raise Exception("No pen or fill makes this useless! Please specify at least one of them as True")
    turtle = Turtle()

    #Store previous delay
    d = delay()

    #Set to draw instantly.
    delay(0)


    turtle.up() #Pickup pen
    turtle.ht() #Hide the turtle
    if (x == None): #Set default value
        x = (0-(size/2))
    if (y == None): #Set default value
        y = (0-(size/2))
    
    turtle.goto(x, y) #Goto co ords
    icM = len(penColor) #Check size of array and limit it.
    if (icM < 1):
        raise Exception("penColor must have at least one entry in array!")

    b = 0 #Store the first color index


    incD =  (360/pieces)#Incrementation of degrees./
    if (useFill):
        turtle.begin_fill()
    if (usePen):
        turtle.down()
        turtle.pensize(penSize)
    for i in range(-1, pieces):
        ic = penColor[b]
        turtle.fillcolor(fillColor)
        turtle.pencolor(ic)
        turtle.circle(size, incD)
        b += 1
        if not(b < icM):
            b = 0
    turtle.end_fill()
    #Reset delay
    delay(d)

    del turtle




def drawTriangle(h, w, x = None, y = None, useFill = True, usePen = True, penColor = "Black", fillColor = "Black", penSize = 1):
    if (x == None): #Set default values
        x = 0-(w/2)
    if (y == None): #Set default values
        y = 0-(h/2)
    else:
        y += (h/2)
    x1, x2 = x-(w/2), x+(w/2)
    y1, y2 = y-(h/2), y+(h/2)
    d = delay()
    delay(0)
    turtle = Turtle()
    turtle.ht()
    turtle.up()
    turtle.goto(x1, y1)
    if (useFill):
        turtle.begin_fill()
        turtle.fillcolor(fillColor)
    if (usePen):
        turtle.down()
        turtle.pencolor(penColor)
    print("x1=",x1,"x2=",x2,"y1=", y1,"y2=", y2,"y=", y,"x=", x) #Debug code
    turtle.goto(x2, y1)
    turtle.goto(x, y2)
    turtle.goto(x1, y1)
    turtle.up()
    turtle.end_fill()
    del turtle
    delay(d)

def drawHouse(size, x = 0, y = 0):
    pass





########This is a test area for use Simply uncomment code
setup()
title("PLACE TITLE HERE")

##Use the api below here to create simple drawings on a window. Or do more complex stuff.
##This is not exactly a proper module so it can not be imported or atleast I do not think. I am still learning python.
##Enjoy!





```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
