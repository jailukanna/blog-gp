---
title: turtle example HappyNewYear (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'HappyNewYear'

Functions in program: 
* `def display_msg(msg):`
* `def change_color(t):`

Modules used in program: 
* `import time`
* `import random`

## python HappyNewYear

Python turtle example: HappyNewYear

```python
from turtle import *
import random
import time

def change_color(t):
    R = random.random()
    B = random.random()
    G = random.random()
    t.pencolor(R, G, B)

def display_msg(msg):
    for c in msg:
        change_color(t)
        t.write(c, move=False, align="left", font=("Arial", 36, "normal"))
        t.forward(50)
    
width = 800
height = 200
xPos = -(width/2)
yPos = 0
setup(width, height)

t = Turtle("turtle")
t.speed(0)
t.hideturtle()
t.begin_fill()
#t._delay(25)
t.penup()

while True:
    t.goto(xPos + 10, yPos)
    display_msg("祝各位新年快樂，豬事順利！！！")
    t.goto(xPos + 10, yPos - 60)
    display_msg("2019/2/4 廖柄㷍")
    time.sleep(2)
    t.clear()

t.end_fill()
done()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
