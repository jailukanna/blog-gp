---
title: turtle example PythonEvents (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'PythonEvents'

Functions in program: 
* `def draw(x, y):`
* `def set_paiting():`
* `def clearScreen():`
* `def setRed():`
* `def ondrag(fun):`
* `def goto(x, y):`
* `def draw(x, y):`
* `def func(x, y):`

## python PythonEvents

Python turtle example: PythonEvents

```python
from turtle import *

# создание и настройка
t = Turtle()
t.color('blue')
t.width(5)
t.shape('circle')
t.pendown()

def func(x, y):
    print(x)
    print(y)

t.onclick(func)

# обработчик
def draw(x, y):
    t.goto(x, y)

# обработчик
def goto(x, y):
    t.penup()
    t.goto(x, y)
    t.pendown()

def ondrag(fun):
    fun(10, 10)

# привязка обработчика к событию черепашки ondrag
# мышь зажата на черепашке и двигается
t.ondrag(t.goto)

# экран
scr = Screen()

# привязка к клику по экрану
scr.onscreenclick(move)

# несколько действий на одно событие
scr.onscreenclick(goto)

def setRed():
    t.color('red')
    
def clearScreen():
    t.clear()

# привязка к нажатию клавиши
# клавиши:
# АНГЛИЙСКИЕ буквы
# "специальные клавиши" - space, up/down/left/right
scr.onkey(setRed,'r')
scr.onkey(clearScreen, 'space')

is_painting = False
def set_paiting():
    if is_painting:
        is_painting = False
    else:
        is_painting = True

# обработчик
def draw(x, y):
    if is_painting == True:
        t.pendown()
    else:
        t.penup()
    t.goto(x, y)
scr.onkey(set_paiting, 'space')

# начинаем слушать все события
scr.listen()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
