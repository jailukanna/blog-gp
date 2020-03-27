---
title: turtle example star (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'star'


## python star

Python turtle example: star

```python
from turtle import *
speed("fastest")
ht()
for i in range(2):
    def s(side,angle):
        '''(integers) 
        -
        > None
        Draws a spiral with angle specified turns and
        lines up to the length specified in side.
        >>> side(100)
        >>> spiral(3000,145)
        >>> pu()
        >>> goto(0,0)
        >>> pd()
        >>> spiral(3000,-145)
        
        '''
        for sidelen in range(1, side+1, 5):
            forward(sidelen)
            right(angle)
        return None

    si=int(input("Input a value for a side: "))
    an=int(input("Input an angle: "))
    s(si,an)
    pu()
    goto(0,0)
    pd()
    s(si,-an)
    pu()
    goto(0,0)
    pd()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
