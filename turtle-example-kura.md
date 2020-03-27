---
title: turtle example kura (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'kura'

Functions in program: 
* `def tree(s):`
* `def snow(s):`
* `def star(s):`
* `def segi(n, s):`

## python kura

Python turtle example: kura

```python
from turtle import *

def segi(n, s):
    for i in range(n):
        forward(s)
        left(360/n)

def star(s):
    for i in range(12):
        forward(s)
        right(150)

def snow(s):
    if s < 3:
        return
    for i in range(6):
        forward(s)
        snow(s/3)
        back(s)
        right(60)

def tree(s):
    if s < 3:
        return
    else:
        pensize(s/20)
        forward(s)
        left(24)        # dahan kiri
        tree(s/2)
        right(48)       # dahan kanan
        tree(s/2)
        left(24)        # kembali ke awal
        back(s)

speed(0)
penup()
goto(0, -200)
pendown()
left(90)

color('green')
tree(200)
done()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
