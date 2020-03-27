---
title: turtle example balapkura (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'balapkura'


## python balapkura

Python turtle example: balapkura

```python
#!/bin/python3

from turtle import*
from random import randint

speed(100)
penup()
goto(-140, 140)

for step in range(15):
  write(step, align="center")
  right(90)
  forward(10)
  pendown()
  forward(150)
  penup()
  backward(160)
  left(90)
  forward(20)
  
a = Turtle()
a.color("red")
a.shape("turtle")
a.penup()
a.goto(-160, 110)
a.pendown()

b = Turtle()
b.color("blue")
b.shape("turtle")
b.penup()
b.goto(-160, 80)
b.pendown()

c = Turtle()
c.color("medium sea green")
c.shape("turtle")
c.penup()
c.goto(-160, 50)
c.pendown()

d = Turtle()
d.color("black")
d.shape("turtle")
d.penup()
d.goto(-160, 20)
d.pendown()

e = Turtle()
e.color("dark goldenrod")
e.shape("turtle")
e.penup()
e.goto(-160, -10)
e.pendown()

for turn in range(1):
  a.right(360)
  b.right(360)
  c.right(360)
  d.right(360)
  e.right(360)

for turn in range(100):
  a.forward(randint(1,5))
  b.forward(randint(1,5))
  c.forward(randint(1,5))
  d.forward(randint(1,5))
  e.forward(randint(1,5))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
