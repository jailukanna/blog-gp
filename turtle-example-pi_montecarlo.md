---
title: turtle example pi montecarlo (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'pi montecarlo'

Functions in program: 
* `def estimar_pi(n, scale, off_x, off_y):`

## python pi montecarlo

Python turtle example: pi montecarlo

```python
# -*- coding: utf-8 -*-
from random import random
from math import sqrt
from turtle import *

speed(0) # Máxima velocidad
n = 100  # Número de intentos
scale = 200 # Proporción del dibujo, por defecto es 200
pos = [(-1.5,.5),(0,.5),(-1.5,-1),(0,-1)] # Posición de los gráficos
font = ('Arial', scale/20, 'bold')
template = "Dentro: {}\nIntentos: {}\nπ = {}"

def estimar_pi(n, scale, off_x, off_y):
    goto(off_x, off_y)
    # dibujar cuadrado
    down()
    for i in range(4):
        forward(scale)
        left(90)
    # dibujar semicírculo
    up()
    forward(scale)
    left(90)
    down()
    circle(scale, 90)
    # dibujar puntos de intentos
    shape("circle")
    shapesize(.5,.5,.5)
    inside = 0
    for i in range(0,n):
        up()
        x=random()
        y=random()
        if sqrt(x*x + y*y) <= 1:
            inside += 1.
            color('green')
        else:
            color('red')
        goto(x*scale + off_x, y*scale + off_y)
        down()
        stamp()
        up()
    # estimar pi
    pi = 4 * (inside / n)
    print(pi)
    # escribir resultado
    goto(0 + off_x,-.3*scale + off_y)
    hideturtle()
    down()
    color('black')
    write(template.format(inside, n, pi), font=font)
    return(pi)

if __name__ == "__main__":
    pi = []
    for i in range(4):
        up()
        home()
        pi.append(estimar_pi(n, scale, pos[i][0]*scale, pos[i][1]*scale))
    up()
    goto(-1 * scale,-1.75 * scale)
    down()
    write('π promedio: {}'.format(sum(pi) / 4), font=('Arial', scale/10, 'bold'))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
