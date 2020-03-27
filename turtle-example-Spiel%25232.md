---
title: turtle example Spiel%25232 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Spiel%25232'

Functions in program: 
* `def setze_block():`
* `def onkey_right():`
* `def onkey_left():`
* `def onkey_up():`
* `def teste_spielfeld():`
* `def teste_ziel():`
* `def gestalte_spielfeld():`
* `def gestalte_spielfiguren():`

## python Spiel%25232

Python turtle example: Spiel%25232

```python
from turtle import *
from random import randint

def gestalte_spielfiguren():
    register_shape("hero.gif")
    register_shape("block.gif")
    block.shape("block.gif")
    hero.shape("hero.gif")
    block.up()
    hero.up()
    title("Willkommen zum Spiel")
    up()
    goto(20,130)
    write("Fang die Kuh !")


def gestalte_spielfeld():
    """ Spielfeld und Figur gestalten """
    bgcolor("green")
    up()
    goto(-10,-10)
    down()
    goto(110,-10)
    goto(110,110)
    goto(-10,110)
    goto(-10,-10)
    hideturtle()
    

def teste_ziel():
    if round(hero.xcor()) == round(block.xcor()) and round(hero.ycor()) == round(block.ycor()):
        hero.write("Juhu")
        hero.undo()
        setze_block()

        
def teste_spielfeld():
    if round(hero.xcor()) < 0 or round(hero.xcor()) > 100:
        hero.backward(10)
        hero.write("AUA!")
        hero.undo()
    if round(hero.ycor()) < 0 or round(hero.ycor()) > 100:
        hero.backward(10)
        hero.write("AUA!")
        hero.undo()


def onkey_up():
    hero.forward(10)
    teste_spielfeld()
    teste_ziel()


def onkey_left():
    hero.left(90)


def onkey_right():
    hero.right(90)


def setze_block():
    blockxy = (
        randint(0, 10) * 10,  # x
        randint(0, 10) * 10   # y
    )
    block.goto(blockxy[0], blockxy[1])


block=Turtle()
hero=Turtle()
gestalte_spielfiguren()
gestalte_spielfeld()
setze_block()
onkey(onkey_up, "Up")
onkey(onkey_left, "Left")
onkey(onkey_right, "Right")
listen()
mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
