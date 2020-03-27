---
title: turtle example shape (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'shape'


## python shape

Python turtle example: shape

```python
from random import *
from turtle import *

hideturtle()
tracer(False)
colormode(255)

sides = randint(3, 10)

for i in range(0, sides):   
    r = randint(0, 255)
    g = randint(0, 255)
    b = randint(0, 255)
    pencolor(r, g, b)
    forward(50)
    left(360./sides)
    update()
Screen().exitonclick()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
