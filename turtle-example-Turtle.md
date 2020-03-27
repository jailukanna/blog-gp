---
title: turtle example Turtle (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Turtle'


## python Turtle

Python turtle example: Turtle

```python

#  Playing around with Turtle Graphics:
#  https://coolpythoncodes.com/python-turtle/

from turtle import Turtle

#  Initialize turtle
t=Turtle()
t.showturtle()
t.screen.bgcolor('black')
t.color('green')
t.shape('turtle')

#  Make a spiral
forward = 0
while (forward < 150):
    forward += 1
    t.fd(forward)
    t.rt(25)

t.screen.exitonclick()
t.screen.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
