---
title: turtle example randomTurtles (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'randomTurtles'

Functions in program: 
* `def maktIMove(valTurtle, step):`
* `def eventRand():`

Modules used in program: 
* `import datetime`

## python randomTurtles

Python turtle example: randomTurtles

```python
from turtle import *
from random import randint
from random import random
import datetime
from construct.createTurtles import gencolor


def eventRand():
    Bruijn = [0, 0, 0, 0, 1, 0, 0, 1, 0, 1, 1, 0, 0, 1, 1, 1, 1, 1, 0, 0, 0, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1]
    i = randint(0, 26)
    X = []
    for o in range(8):
        Bitarr1 = [Bruijn[i], Bruijn[i - 1], Bruijn[i - 2], Bruijn[i - 3], Bruijn[i - 4]]
        S = Bitarr1[0] * 2 + Bitarr1[1]
        T = Bitarr1[2] * 4 + Bitarr1[3] * 2 + Bitarr1[4] * 1
        i = i - 1
        X.append(S)
        X.append(T)
    return X


def maktIMove(valTurtle, step):
    event = eventRand()
    min_s = 1
    max_s = 3
    turRoadLengh = [0, 0, 0, 0]
    currentDT = datetime.datetime.now()
    time_r = [datetime.datetime.now(), datetime.datetime.now(), datetime.datetime.now(), datetime.datetime.now()]
    road_l = step * 20
    flag = [1, 1, 1, 1]
    tur_set = [100, 70, 40, 10]
    num_e = randint(1, road_l/2)
    num_f = randint(num_e, road_l)
    i = 0
    while(flag[0] == 1 or flag[1] == 1 or flag[2] == 1 or flag[3] == 1):
        i = i + 1
        speed = [randint(min_s, max_s), randint(min_s, max_s), randint(min_s, max_s), randint(min_s, max_s)]
        for j in range(4):

            if (turRoadLengh[j] < 20 * step and flag[j] >= 1):
                if ((1 == event[2 * j + 1] or event[2 * j + 1] == 2) or (1 == event[4 * j + 1] or event[4 * j + 1] == 2)) and (i == num_e):
                    if (1 == event[4 * j + 1] or event[4 * j + 1] == 2):
                        e_index = event[4*j]
                    else:
                        e_index = event[2*j]
                    valTurtle[e_index].right(180)
                    valTurtle[e_index].forward(speed[e_index]*4)
                    valTurtle[e_index].right(180)
                    road_l -= speed[e_index]*3
                if ((3 == event[2 * j + 1] or event[2 * j + 1] == 4) or (3 == event[4 * j + 1] or event[4 * j + 1] == 4)) and (num_e < i < num_f):
                    speed[event[2 * j]] = 0
                    if (3 == event[4 * j + 1] or event[4 * j + 1] == 4):
                        e_index = event[4*j]
                    else:
                        e_index = event[2*j]
                    valTurtle[e_index].color(gencolor())
                turRoadLengh[j] += speed[j]
                valTurtle[j].forward(speed[j])
            else:
                flag[j] = flag[j] - 1
                valTurtle[j].goto(-150 + 20 * step, tur_set[j])
                if (flag[j]):
                    time_r[j] = datetime.datetime.now()
        road_l -= min(speed)
    valTurtle[0].write(currentDT.microsecond - time_r[0].microsecond)
    valTurtle[1].write(currentDT.microsecond - time_r[1].microsecond)
    valTurtle[2].write(currentDT.microsecond - time_r[2].microsecond)
    valTurtle[3].write(currentDT.microsecond - time_r[3].microsecond)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
