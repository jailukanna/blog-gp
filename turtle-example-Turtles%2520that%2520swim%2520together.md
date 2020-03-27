---
title: turtle example Turtles%2520that%2520swim%2520together (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Turtles%2520that%2520swim%2520together'


## python Turtles%2520that%2520swim%2520together

Python turtle example: Turtles%2520that%2520swim%2520together

```python
from turtle import *        # use the turtle library
space = Screen()            # create a ocean for the turtles to swim in
zari = Turtle()             # create a turtle named zari
jim = Turtle()              # create a turtle named jim
turtles = []                # create a turtle home
turtles.append(jim)         # add jim the turtle to the home
turtles.append(zari)        # add zari the turtle to the home
jim.up()                    # 
jim.setheading(90)          # 
jim.forward(90)             # These lines seperate the turtles
jim.setheading(0)           # So you can see the work you have done
jim.down()                  #

for obj in turtles:
    obj.speed(1)            # slower speed for easier understanding
    obj.forward(100)        # move the turtles together
    
#opional if you would like to put the turtles back, see below

for obj in turtles:
    obj.setheading(180)
    obj.forward(100)

  
jim.up()
jim.setheading(270)
jim.forward(90)
jim.setheading(180)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
