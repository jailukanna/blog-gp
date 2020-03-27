---
title: turtle example Pie Chart Drawer (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Pie Chart Drawer'

Functions in program: 
* `def drawChart():`
* `def inputChart():`

## python Pie Chart Drawer

Python turtle example: Pie Chart Drawer

```python
from turtle import *

data = {}
speed(0)
slices = 0
total = 0

def inputChart():
    global slices,total
    try:
        slices = int(input('No. of slices: '))
    except:
        inputChart()
    for i in range(slices):
        data[str(i)]=input('<name> <frequency> <colour>: ')
    total = 0
    for i in range(len(data)):
        total += int(data[str(i)].split()[1])
    print('Total: {}'.format(total))
    drawChart()
        

def drawChart():
    global slices,total
    circle(100)
    left(90)
    penup()
    forward(100)
    pendown()
    position = pos()
    right(90)
    forward(100)
    setpos(position)

    a = 360 / slices
    print(a)
    for i in range(slices):
        left(((int(data[str(i)].split()[1]) / total) * 360) / 2)
        position = pos()
        color(data[str(i)].split()[2])
        penup()
        forward(50)
        pendown()
        write(data[str(i)].split()[0],move=False,font=('Arial',12))
        penup()
        setpos(position)
        color('black')
        
        pendown()
        left(((int(data[str(i)].split()[1]) / total) * 360) / 2)
        positon = pos()
        forward(100)
        setpos(position)

    
inputChart()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
