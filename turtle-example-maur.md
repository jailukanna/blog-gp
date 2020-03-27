---
title: turtle example maur (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'maur'


## python maur

Python turtle example: maur

```python
from turtle import *
black = set()
speed(11)
while(True):
    x,y = position()
    pos = (round(x),round(y))
    if (pos in black):
        black.remove(pos)
        right(90)
    else:
        black.add(pos)
        left(90)    
    forward(5)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
