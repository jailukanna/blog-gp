---
title: turtle example 4 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example '4'

Functions in program: 
* `def main():`
* `def yin(radius, color1, color2):`

## python 4

Python turtle example: 4

```python
#!/usr/bin/env python3
"""       turtle-example-suite:

            tdemo_yinyang.py

Another drawing suitable as a beginner's
programming example.

The small circles are drawn by the circle
command.

"""

from turtle import *

def yin(radius, color1, color2):
    width(3)
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
    reset()
    yin(100, "black", "white")
    yin(100, "white", "black")
    ht()
    up()
    goto(-100,200)
    yin(100, "black", "white")
    yin(100, "white", "black")
    ht()
    up()
    goto(+100,200)
    yin(100, "black", "white")
    yin(100, "white", "black")
    ht()
    return "Done!"

if __name__ == '__main__':
    main()
    mainloop()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
