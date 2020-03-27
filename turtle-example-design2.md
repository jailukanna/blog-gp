---
title: turtle example design2 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'design2'


## python design2

Python turtle example: design2

```python
from turtle import *

from random import randint
speed(0)

bgcolor('black')

x = 1

while x < 400:

    r = randint(0,255) 
    g = randint(0,255) 
    b = randint(0,255) 

    colormode(255) 
    pencolor(r,g,b)

    fd(50 + x)
    rt(90.911)


    x = x+1 

exitonclick() 

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
