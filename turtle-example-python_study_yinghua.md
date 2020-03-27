---
title: turtle example python study yinghua (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'python study yinghua'

Functions in program: 
* `def main():`
* `def petal(m,t):`
* `def tree(branchLen,t):`

Modules used in program: 
* `import random`
* `import turtle`

## python python study yinghua

Python turtle example: python study yinghua

```python
import turtle
import random
from turtle import *
from time import sleep

def tree(branchLen,t):
    sleep(0.0005)
    if branchLen > 3:
        if 8<= branchLen <= 12:
            if random.randint(0,2) == 0:
                t.color('snow')
            else:
                t.color('lightcoral')
            t.pensize(branchLen / 3)
        elif branchLen < 8:
            if random.randint(0,1) == 0:
                t.color('snow')
            else:
                t.color('lightcoral')
            t.pensize(branchLen / 2)
        else:
            t.color('sienna')
            t.pensize(branchLen / 10)
        t.forward(branchLen)
        a = 1.5 * random.random()
        t.right(20 * a)
        b = 1.5 * random.random()
        tree(branchLen - 10 * b,t)
        t.left(40*a)
        tree(branchLen - 10 * b,t)
        t.right(20*a)
        t.up()
        t.backward(branchLen)
        t.down()

def petal(m,t):
    for i in range(m):
        a = 200 - 400 * random.random()
        b = 10 - 20 * random.random()
        t.up()
        t.forward(b)
        t.left(90)
        t.forward(a)
        t.down()
        t.color('lightcoral')
        t.circle(1)
        t.up()
        t.backward(a)
        t.right(90)
        t.backward(b)

def main():
    t = turtle.Turtle()
    w = turtle.Screen()
    t.hideturtle()
    getscreen().tracer(5,0)
    w.screensize(bg = 'wheat')
    t.left(90)
    t.up()
    t.backward(150)
    t.down()
    t.color('sienna')

    tree(60,t)
    petal(200,t)
    w.exitonclick()

main()





        


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
