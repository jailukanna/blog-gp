---
title: turtle example koch (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'koch'

Functions in program: 
* `def drawKoch(n, level):`

## python koch

Python turtle example: koch

```python
from turtle import *

colors = ['#ff0000', '#00ff00', '#003366', '#ff6600', '#6600cc']

def drawKoch(n, level):
    begin_fill()
    if level == 0:
        forward(n)
        lt(60)
        forward(n)
        rt(120)
        forward(n)
        lt(60)
        forward(n)
    else:
        drawKoch(n/3, level-1)
        lt(60)
        drawKoch(n/3, level-1)
        rt(120)
        drawKoch(n/3, level-1)
        lt(60)
        drawKoch(n/3, level-1)
    end_fill()

speed('fastest')
for i in range(5):
    color(colors[i%len(colors)])
    penup()
    goto((-450, 0))
    pendown()
    drawKoch(300, i)
    lt(180)
    drawKoch(300, i)
    lt(180)
done()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
