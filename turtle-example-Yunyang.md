---
title: turtle example Yunyang (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Yunyang'

Functions in program: 
* `def main():`
* `def yin(radius, color1, color2):`

## python Yunyang

Python turtle example: Yunyang

```python
#!/usr/bin/env python3
"""
M0329007 邱思綺
"""

from turtle import *

def yin(radius, color1, color2):
    width(2)
    color("black", color1)
    begin_fill()
    circle(radius/2., 180)
    circle(radius, 180)
    left(180)
    circle(-radius/2., 180)
    end_fill()
    left(90)
    up()
    forward(radius*0.35)
    right(90)
    down()
    color(color1, color2)
    begin_fill()
    circle(radius*0.15)
    end_fill()
    left(90)
    up()
    backward(radius*0.35)
    down()
    left(90)

def main():
    speed(0)
    yin(100, "black", "white")
    yin(100, "white", "black")
    up()
    L= 200
    goto(+L,+L);
    down()
    yin(100, "black", "white")
    yin(100, "white", "black")
    up()
    goto(-L,-L);
    down()
    yin(100, "black", "white")
    yin(100, "white", "black")
    return "Done!"

if __name__ == '__main__':
    main()
    mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
