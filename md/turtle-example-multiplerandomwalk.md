---
title: turtle example multiplerandomwalk (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'multiplerandomwalk'

Functions in program: 
* `def starttowalk():`

Modules used in program: 
* `import random`
* `import numpy as np`

## python multiplerandomwalk

Python turtle example: multiplerandomwalk

```python
import numpy as np
from turtle import *
import random


turtles = 5
screensize(500,500)

class walker():
    def __init__(self, colo, pos):
        self.pos = pos
        self.tur = Turtle()
        self.tur.shape('turtle')
        self.tur.setpos(pos)
        self.tur.speed(30)
        self.tur.color(colo)
        self.tur.penup()
        self.tur.setheading(90)
 
    def walking(self):   
        choice = np.random.randint(4, size=1)[0]
        actionofchoice = [(self.pos[0]+5, self.pos[1]), (self.pos[0], self.pos[1]+5), (self.pos[0]-5, self.pos[1]), (self.pos[0], self.pos[1]-5)]
        headsup = [90, 180, 270, 360]
        self.pos = actionofchoice[choice]
        self.tur.right(headsup[choice])
        self.tur.pendown()
        self.tur.goto(self.pos)
        

def starttowalk():
    allwalker = []
    colors = ['red','yellow','mediumturquoise','blue','orange']
    for x in range(turtles):
        allwalker.append(walker(colors[x], (0,0)))
        allwalker[x].tur.showturtle()

    while True:
        for x in allwalker:
            x.walking()

if __name__ == "__main__":
    starttowalk()
  




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
