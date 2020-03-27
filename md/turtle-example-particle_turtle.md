---
title: turtle example particle turtle (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'particle turtle'


Modules used in program: 
* `import random as r`

## python particle turtle

Python turtle example: particle turtle

```python
from turtle import *
import random as r

RADIUS = 50

# visual of the circle
c = Turtle()
c.penup()
c.setpos(c.pos()[0], c.pos()[1]-RADIUS)
c.pendown()
c.color('blue')
c.circle(RADIUS)

# particle
t = Turtle()
t.color('red')

# main loop
count = 0
while True:
    t.forward(5)
    if r.randint(0, 1) == 0:
        t.left(r.randint(0, 100))
    else:
        t.right(r.randint(0, 100))
    if abs(t.pos()[0]) >= RADIUS or abs(t.pos()[1]) >= RADIUS:
        break
    count += 1
done()

# print(out the number of moves the particle made)
print(str(count))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
