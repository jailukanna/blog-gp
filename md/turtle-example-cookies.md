---
title: turtle example cookies (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'cookies'

Functions in program: 
* `def draw_cookie(x, y):`

Modules used in program: 
* `import random`

## python cookies

Python turtle example: cookies

```python
# https://docs.python.org/3.4/library/turtle.html
from turtle import *
import random

def draw_cookie(x, y):
    penup()
    goto(x,y)
    pendown()
    radius = random.randint(100,200)
    col = random.choice(['#CC6600','#FFCD82','#8F4700'])
    color(col)
    dot(radius)
    for i in range(random.randint(15,20)):
        size = int(radius/3)
        x2 = x + random.randint(-size,size)
        y2 = y + random.randint(-size,size)
        col2 = random.choice(['#663300','#754719','#996633'])
        color(col2)
        radius2 = random.randint(10,20)
        penup()
        goto(x2, y2)
        pendown()
        dot(radius2)

draw_cookie(10, 10)
onscreenclick(draw_cookie)
done()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
