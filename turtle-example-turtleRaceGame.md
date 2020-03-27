---
title: turtle example turtleRaceGame (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtleRaceGame'


## python turtleRaceGame

Python turtle example: turtleRaceGame

```python
#!/bin/python3

from turtle import *
from random import randint

speed(0)
penup()
goto(-140, 140)

for step in range(15):
  write(step, align='center')
  right(90)
  for num in range(8):
    penup()
    forward(10)
    pendown()
    forward(10)
  penup()
  backward(160)
  left(90)
  forward(20)

a = Turtle()
a.color('red')
a.shape('turtle')

a.penup()
a.goto(-160, 100)
a.pendown()

for turn in range(10):
  a.right(36)

b = Turtle()
b.color('blue')
b.shape('turtle')

b.penup()
b.goto(-160, 70)
b.pendown()

for turn in range(72):
  b.left(5)

c = Turtle()
c.shape('turtle')
c.color('green')

c.penup()
c.goto(-160, 40)
c.pendown()

for turn in range(60):
  c.left(6)

d = Turtle()
d.shape('turtle')
d.color('cyan')

d.penup()
d.goto(-160, 10)
d.pendown()

for turn in range(30):
  d.right(12)

for turn in range(100):
  a.forward(randint(1,5))
  b.forward(randint(1,5))
  c.forward(randint(1,5))
  d.forward(randint(1,5))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
