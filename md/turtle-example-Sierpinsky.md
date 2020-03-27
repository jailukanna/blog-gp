---
title: turtle example Sierpinsky (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Sierpinsky'

Functions in program: 
* `def s(l, x, y):`

Modules used in program: 
* `import math`

## python Sierpinsky

Python turtle example: Sierpinsky

```python

import math
from turtle import * 

size = 800
minimum = 8
pythagoras = math.sqrt(3) / 2

def s(l, x, y):
    if l>minimum:
        l = l/2
        s(l, x, y)
        s(l, x+l, y)
        s(l, x+l/2, y+l*pythagoras)
    else:
        goto(x, y)
        pendown()
        begin_fill()
        forward(l)
        left(120)
        forward(l)
        left(120)
        forward(l)
        end_fill()
        setheading(0)
        penup()
        goto(x,y)
penup()
speed('fastest')
s(size, -size/2, -size*pythagoras/2.0)
done()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
