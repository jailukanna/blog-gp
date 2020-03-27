---
title: turtle example HW1 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'HW1'

Functions in program: 
* `def main():`

## python HW1

Python turtle example: HW1

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
                   "seagreen4", "orchid4",
                   "royalblue1", "dodgerblue4")

    reset()
    Screen()
    up()
    goto(-320,-195)
    width(70)

    for pcolor in peacecolors:
        color(pcolor)
        down()
        forward(640)
        up()
        backward(640)
        left(180)
        forward(66)
        right(180)

    width(25)
    
    
    
    
    color("Black")
    goto(0,-170)
    down()

    circle(170)
    left(90)
    forward(340)
    up()
    left(180)
    forward(170)
    right(45)
    down()
    forward(170)
    up()
    backward(170)
    left(90)
    down()
    forward(170)
    up()

    goto(0,300) 
   

    return "Done!"

if __name__ == "__main__":
    main()
    mainloop()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
