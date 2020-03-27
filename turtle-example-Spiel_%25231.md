---
title: turtle example Spiel %25231 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Spiel %25231'

Functions in program: 
* `def block ():`
* `def r():`
* `def l():`
* `def f():`
* `def teste_spielfeld():`
* `def teste_ziel():`
* `def spielfeld():`

## python Spiel %25231

Python turtle example: Spiel %25231

```python
from turtle import *
from random import *
shape("turtle")
def spielfeld():
    pass
    #Spielfeld gestalte
    #Figur gestalten
def teste_ziel():
    print(round(xcor()),round(ycor()))
    if round(xcor())==x and round(ycor())==y:
        write("Juhu")
        clear()
        spielfeld()
        block()
def teste_spielfeld():
    if round(xcor())<0 or round(xcor())>100:
        backward(10)
        write("AUA!")
        undo()
    if round(ycor())<0 or round(ycor())>100:
        backward(10)
        write("AUA!")
        undo()
def f():
    fd(10)
    teste_spielfeld()
    teste_ziel()    
def l():
    left(90)
def r():
    right(90)
def block ():
    global x,y
    x=randint(0,10)*10
    y=randint(0,10)*10
    up()
    goto(x,y)
    dot(10)
    goto(0,0)

spielfeld()
block()
onkey(f,"Up")
onkey(l,"Left")
onkey(r,"Right")
listen()
print(x,y)
mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
