---
title: turtle example PyHW Paint (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'PyHW Paint'

Functions in program: 
* `def main():`
* `def changecolor(x=0, y=0):`
* `def switchupdown(x=0, y=0):`

## python PyHW Paint

Python turtle example: PyHW Paint

```python
#!/usr/bin/env python3
"""
"""
from turtle import *

def switchupdown(x=0, y=0):
    if pen()["pendown"]:
        end_fill()
        up()
    else:
        down()
        begin_fill()

def changecolor(x=0, y=0):
    global colors
    colors = colors[1:]+colors[:1]
    color(colors[0])

def main():
    global colors
    shape("circle")
    resizemode("user")
    shapesize(1)
    width(3)
    colors=["black", "red", "green", "blue", "yellow", "white"]
    color(colors[0])
    switchupdown()
    onscreenclick(goto,1)
    onscreenclick(changecolor,2)
    onscreenclick(switchupdown,3)
    return "EVENTLOOP"

if __name__ == "__main__":
    msg = main()
    print(msg)
    mainloop()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
