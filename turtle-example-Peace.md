---
title: turtle example Peace (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Peace'

Functions in program: 
* `def main():`

## python Peace

Python turtle example: Peace

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

def main():
    peacecolors = ("red3",  "orange", "yellow",
                   "seagreen4", "royalblue1",
                   "dodgerblue4", "orchid4")

    reset()
    Screen()
    up()
    goto(-320,-195)
    width(60)

    for pcolor in peacecolors:
        color(pcolor)
        down()
        forward(640)
        up()
        backward(640)
        left(90)
        forward(60)
        right(90)

    width(20)
    i=170
    color("black")
    goto(0,-180)
    down()

    circle(i)
    left(90)
    forward(340)
    up()
    left(180)
    forward(i)
    right(45)
    down()
    forward(i)
    up()
    backward(i)
    left(90)
    down()
    forward(i)
    up()

    goto(0,300) # vanish if hideturtle() is not available ;-)
    return "Done!"

if __name__ == "__main__":
    main()
    mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
