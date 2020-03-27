---
title: turtle example turtlex3 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtlex3'


## python turtlex3

Python turtle example: turtlex3

```python
from turtle import *
t=Turtle()
t.color("red","green")
t.hideturtle()
t.write("sajid ansari" ,move=True,align="center",font=("Freestyle Script",100,"normal"))
t.penup()
t.right(100)
t.forward(20)
t.right(80)
t.forward(200)
t.left(90)
t.forward(150)
t.pendown()

t.screen.bgcolor("black")
colors=["red","yellow","purple"]
t.screen.tracer(0,0)
for x in range(100):
    t.circle(x)
    t.color(colors[x%3])
    t.left(60)
t.screen.exitonclick()
t.screen.mainloop()
done()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
