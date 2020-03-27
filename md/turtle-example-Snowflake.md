---
title: turtle example Snowflake (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Snowflake'

Functions in program: 
* `def fullflake():`
* `def snowflake(length, line=forward, *crinkle):`

## python Snowflake

Python turtle example: Snowflake

```python
from turtle import *

speed(0)

#Try walking this snowflake pattern on the carpet before you code it.
#Try drawing it on paper.
#Try drawing it...using smaller versions of itself.

def snowflake(length, line=forward, *crinkle):
    line(length/3, *crinkle)
    right(60)
    line(length/3, *crinkle)
    left(120)
    line(length/3, *crinkle)
    right(60)
    line(length/3, *crinkle)
    left(120)
    line(length/3, *crinkle)
    right(60)
    line(length/3, *crinkle)
    left(120)
    line(length/3, *crinkle)
    right(60)
    line(length/3, *crinkle)
    left(120)
    line(length/3, *crinkle)
    right(60)
    line(length/3, *crinkle)
    left(120)
    line(length/3, *crinkle)
    right(60)
    line(length/3, *crinkle)
    left(120)
    line(length/3, *crinkle)
    right(60)
    line(length/3, *crinkle)
    left(120)
    line(length/3, *crinkle)
    right(60)
    line(length/3, *crinkle)
    left(120)

#Drawing the pattern three times makes the full snowflake.
def fullflake():
    for i in range(0,3):
        snowflake(400, snowflake)
        left(120)

#Display settings
speed(0)
Screen().setup(800,800)



#Starting point for drawing
penup()
goto(-200,-100)
pendown()

#Here, put your code for actually drawing.
#This is a sample.  You can use fullflake() too.
snowflake(10000, snowflake, snowflake, snowflake, snowflake, snowflake)
hideturtle()
Screen().exitonclick()

# Credit to:
# http://www.algorithm.co.il/blogs/computer-science
# /fractals-in-10-minutes-no-6-turtle-snowflake/


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
