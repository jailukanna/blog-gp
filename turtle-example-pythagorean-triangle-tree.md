---
title: turtle example pythagorean-triangle-tree (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'pythagorean-triangle-tree'

Functions in program: 
* `def main():`
* `def square(a, b, level):`

Modules used in program: 
* `import math`

## python pythagorean-triangle-tree

Python turtle example: pythagorean-triangle-tree

```python
from turtle import *
import math

def square(a, b, level):
    if level == 0:
        return

    side = math.sqrt((a[0] - b[0]) ** 2 + (a[1] - b[1]) ** 2)
    angle = math.asin(0.6)
    direction = math.atan2(b[1] - a[1], b[0] - a[0])

    c = (b[0] - b[1] + a[1], b[0] + b[1] - a[0])
    d = (a[0] + a[1] - b[1], b[0] - a[0] + a[1])
    e = (d[0] + (math.sin(angle - direction) * side * 0.6), d[1] + (math.cos(angle - direction) * side * 0.6))

    scale = 200 - 10 * level

    hideturtle()
    width(1)
    color(scale, scale, scale)
    speed(0)
    begin_fill()
    penup()
    goto(*a)
    pendown()
    left(math.degrees(direction))
    forward(side)
    left(90)
    forward(side)
    left(90)
    forward(side)
    left(90)
    forward(side)
    left(90 - math.degrees(direction))
    end_fill()

    square(e, c, level - 1)
    square(d, e, level - 1)

def main():
    colormode(255)
    square((-120, -200), (0, -200), 7)

if __name__ == '__main__':
    main()
    mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
