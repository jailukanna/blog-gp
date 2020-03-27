---
title: turtle example gravity order1 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'gravity order1'


## python gravity order1

Python turtle example: gravity order1

```python
# a simple python example for SIS at BPS,   bill,  Jan 2017
# using turtle graphics 
#
# simulate the effect of gravity on ball thrown upwards
# using first order equations
#


from turtle import *

# setup variables for time, x step, gravity, initial velocity 
t=0
tstep = 0.25    # seconds   
delx = 2        # uniform motion in x axis 
g = -1          # fixed accel (downwards) 
yvel = 20       # initial vel in y 
y=0             # y position 

# draw a cliff that the ball can be thrown from 
penup()
goto(-100,0)
pendown()
color("green")
forward(150)
right(90)
forward(100)
penup()
goto(0,0)

pendown()
color("blue")

print("run the simulation for 50 seconds ")
# main loop -- uniform motion in x and acceleration in y axis 
while (t<50):
    t=t+tstep
    x=t*delx
    yvel = yvel + g * tstep     # change in y vel = accel * time interval
    y=y + yvel * tstep          # change in y posn = vel + time interval 
    
    goto(x,y)
    dot(2, "blue")              # draw dot at current position 

print("final y position is " + str(y) + " m")
write(" python rules! ")
print("that's all folks")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
