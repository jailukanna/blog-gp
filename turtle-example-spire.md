---
title: turtle example spire (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'spire'


Modules used in program: 
* `import math`

## python spire

Python turtle example: spire

```python
from turtle import *
import math
speed(1)
a = 10
n = 3
for i in range(5):
    penup()
    goto(0,0)
    setheading(0)
    forward(a / (2 * sin(math.radians(180 /n))))
    print(position())
    pendown()
    ang_a = 180 - (180 * (n - 2)) / n
    print(ang_a)
    left(ang_a / 2)
    for k in range(n):
        forward(a)
        left(180 - ang_a)
    r = a / (2 * sin(math.radians(180 / n)))
    n += 1
    r += 10
    a = r * 2 * sin(math.radians(180 / n))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
