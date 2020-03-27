---
title: turtle example Main (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Main'


Modules used in program: 
* `import time`

## python Main

Python turtle example: Main

```python
#Programmer Cahill Howell
#9/12/2019 v1.0.0 (versions change major, minor, little to none change versions)
#Trying to draw with the turtle
import time
from turtle import *
colors = ['red','blue', 'green', 'orange', 'yellow', 'purple']

forward(80), left(90)
forward(30), left(90)
forward(60), left(270)
forward(50), left(270)
forward(60), left(90)
forward(30), left(90)
forward(80), left(90)
forward(120)
time.sleep(0)
#this line of code will draw out the letter C however its forever writing
#kinda like writing consistently without picking up your pen

forward(150), left(90)
forward(50), circle(50)
forward(50), left(90)
forward(100),left(90)
forward(100), left(90)
forward(100)
time.sleep(0)
#these lines will run a circle, then follow a square around it. Circle size
#is like.. circle(X) just as forward() left() right()
right(90)
forward(250), right(90)
forward (300)

forward(100)
for i in range(250):
    forward(i)
    left(91)
time.sleep(2.5)
#little function work here, the i is just an integer, 0 to n, n being any
#number, so this causes the loop, however because i defined it to stop at 250
#it takes a little while to run, at 100(which is the distance for each line)
#but it looks really cool


#successfully added this to the github account

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
