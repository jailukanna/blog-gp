---
title: turtle example HW1 1 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'HW1 1'

Functions in program: 
* `def main():`
* `def yin(radius, color1, color2):`

## python HW1 1

Python turtle example: HW1 1

```python
#!/usr/bin/env python3
"""       turtle-example-suite:

              tdemo_peace.py

A simple drawing suitable as a beginner's
programming example. Aside from the
peacecolors assignment and the for loop,
it only uses turtle commands.
"""

from turtle import *

def yin(radius, color1, color2):
    width(3)
    color(color2, color1)
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
    peacecolors = ("red3",  "darkorange1", "yellow3",
                   "seagreen4", "royalblue1",
                   "midnightblue", "purple1", 
				   "mediumpurple1", "navy",
				   "deepskyblue", "springgreen",
				   "gold", "orange", "red")

    reset()
    up()
    goto(-320,-195)
    width(35)

    for pcolor in peacecolors:
        color(pcolor)
        down()
        forward(640)
        up()
        backward(640)
        left(90)
        forward(33)
        right(90)
	
    up()
    goto(0,22)
    yin(180, "black", "white")
    yin(180, "white", "black")
    ht()
    return "Done!"

if __name__ == "__main__":
    main()
    mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
