---
title: turtle example a (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'a'

Functions in program: 
* `def yen(t,x,y,s) :`

## python a

Python turtle example: a

```python
from turtle import *

def yen(t,x,y,s) :
    t.penup()
    t.setx (x)
    t.sety (y-s)
    t.pendown()
    t.circle(s)
t=Turtle()
s=t.getscreen()
s.register_shape("a.gif")
t.shape('a.gif')
yen(t,0,0,100)
done()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
