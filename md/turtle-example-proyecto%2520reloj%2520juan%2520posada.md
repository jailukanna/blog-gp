---
title: turtle example proyecto%2520reloj%2520juan%2520posada (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'proyecto%2520reloj%2520juan%2520posada'

Functions in program: 
* `def segundos():`
* `def minutos():`
* `def horas():`
* `def hacerreloj():      #hacer reloj`

Modules used in program: 
* `import time`

## python proyecto%2520reloj%2520juan%2520posada

Python turtle example: proyecto%2520reloj%2520juan%2520posada

```python
from turtle import *
import time
t1 = Turtle()
t2 = Turtle()
t3 = Turtle()
t4 = Turtle()
t2.speed(0)
t3.speed(0)
t4.speed(0)
t1.ht()
t2.ht()
t3.ht()
t4.ht()

h = int(input("digite horas"))
m = int(input("digite minutos"))
s = int(input("digite segundos"))
n = 90
y = 270
def hacerreloj():      #hacer reloj
    t1.pensize(8)
    t1.speed(100)
    t1.right(90) 
    t1.pu()
    t1.forward(360)
    t1.left(90)
    t1.pd()
    t1.circle(360,360)
    b = 0
    while b < 12:
        t1.pu()
        t1.pencolor("black")
        t1.circle(360,30)
        t1.left(90)
        t1.forward(15)
        t1.pd()
        t1.forward(30)
        t1.pu()
        t1.left(180)
        
        t1.forward(45)
        t1.left(90)
        t1.pu()
        b += 1

def horas():
    global h
    t4.pensize(12)
    t4.pencolor("black") #horas
    t4.goto(0,0)
    t4.seth(90-h*30)
    t4.pd()
    t4.clear()
    t4.forward(150)
    

def minutos():
    global m
    t3.pensize(8)
    t3.pencolor("green") 
    t3.goto(0,0)
    t3.seth(90 - m * 6)
    t3.pd()
    t3.clear()
    t3.forward(300)
    
    
    

    
def segundos():
    global s
    t2.pensize(5)
    t2.pencolor("red")
    t2.goto(0,0)
    t2.seth(90 - s * 6)
    t2.pd()
    t2.clear()
    t2.forward(300)
    
    
hacerreloj()
horas()
minutos()

while h < 13:
    while m < 61:
        while s < 61:
            t2.clear()
            segundos()
            print("h", h, "m", m, "s", s)
            s += 1
            time.sleep(0.845)
            
        t3.clear()
        minutos()
        m += 1
        s = 1
        
    t4.clear()
    horas()
    h += 1
    m = 0
        


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
