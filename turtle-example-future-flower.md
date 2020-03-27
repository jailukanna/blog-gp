---
title: turtle example future-flower (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'future-flower'


## python future-flower

Python turtle example: future-flower

```python
from random import *
from turtle import *
from time import *

shapes = int(input("How many shapes do you want? "))

hideturtle()
tracer(False)
q=1
colormode(255)

sides = randint(3, 10)

for i in range(0, shapes):
    right(q)
    q+=1
    for i in range(0, sides):   
        pendown()
        r = randint(0, 255)
        g = randint(0, 255)
        b = randint(0, 255)
        pencolor(r, g, b)
        forward(q + 49)
        right(360./sides)
        update()
Screen().exitonclick()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
