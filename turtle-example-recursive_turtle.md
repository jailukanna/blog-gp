---
title: turtle example recursive turtle (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'recursive turtle'

Functions in program: 
* `def draw_sine(x):`

Modules used in program: 
* `import math`
* `import time`

## python recursive turtle

Python turtle example: recursive turtle

```python
#! /usr/bin/python3
# rootVIII
# Make turtle recursively
# draw a sine wave
from turtle import *
import time
import math


def draw_sine(x):
    if not x < 50:
        print("done")
    else:
        y = 30 * math.sin(.5 * x)
        setx(x)
        sety(y)
        penup()
        goto(x, y)
        dot()
        pendown()
        draw_sine(x + .25)

if __name__ == "__main__":
    x = -50
    penup()
    draw_sine(x)
    time.sleep(100)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
