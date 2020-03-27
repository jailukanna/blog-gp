---
title: turtle example PyHW Peace (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'PyHW Peace'

Functions in program: 
* `def main():`

## python PyHW Peace

Python turtle example: PyHW Peace

```python
from turtle import *

def main():
    peacecolors = ("purple",  "orange", "yellow", "seagreen4", "orchid4","royalblue1")

    reset()
    Screen()
    up()
    goto(-60,30)
    width(60)

    for pcolor in peacecolors:
        color(pcolor)
        down()
        forward(300)
        up()
        backward(300)
        left(30)
        forward(60)
        right(90)

    width(10)
    color("red")
    goto(125,50)
    down()

    begin_fill()

    left(90)
    circle(75,180)
    up()
    goto(0,50)
    down()
    left(180)
    circle(75,180)

    goto(0,-150)
    goto(125,50)

    end_fill()

    return "Done!"

if __name__ == "__main__":
    main()
    mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
