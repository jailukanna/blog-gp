---
title: turtle example TurtleProj-master construct randomTurtles (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'TurtleProj-master construct randomTurtles'

Functions in program: 
* `def random(valTurtle, step):`
* `def eventrand():`

Modules used in program: 
* `import datetime`

## python TurtleProj-master construct randomTurtles

Python turtle example: TurtleProj-master construct randomTurtles

```python
from turtle import *
from random import randint
from random import random
from typing import Any, Union
import datetime


def eventrand():
    Bruijn = [0,0,0,0,1,0,0,1,0,1,1,0,0,1,1,1,1,1,0,0,0,1,1,0,1,1,1,0,1,0,1,0,0,0,0]
    i = randint(8,31)
    X = []
    for o in range(8):
        Bitarr1 = [Bruijn[i],Bruijn[i+1],Bruijn[i+2],Bruijn[i+3],Bruijn[i+4]]
        S = Bitarr1[0]*2+Bitarr1[1]
        T = Bitarr1[2]*4 + Bitarr1[3]*2 + Bitarr1[4]*1
        i = i - 1
        X.append(S)
        X.append(T)
    return X

def random(valTurtle, step):
    event = eventrand()
    min_s = 1
    max_s = 3
    turRoadLengh = [0,0,0,0]
    currentDT = datetime.datetime.now()
    time_r = [datetime.datetime.now(),datetime.datetime.now(),datetime.datetime.now(),datetime.datetime.now()]
    road_l = step*12
    flag = [1,1,1,1]
    tur_set = [100,70,40,10]
    num_e = randint(1, road_l)
    num_f = randint(1, road_l)
    for i in range(road_l):
        speed = [randint(min_s,max_s),randint(min_s,max_s),randint(min_s,max_s),randint(min_s,max_s)]
        road_l -= min(speed)
        for j in range(4):
            if (turRoadLengh[j] < 20 * step and flag[j] >= 1):
                if ((1 == event[2*j + 1] or event[2*j + 1] == 2)and(i == num_e))or((1 == event[4*j + 1] or event[4*j + 1] == 2)and(i == num_f)):
                    valTurtle[event[2*j]].right(180)
                    valTurtle[event[2*j]].forward(speed[event[2*j]]*4)
                    valTurtle[event[2*j]].right(180)
                    turRoadLengh[event[2*j]] -= speed[event[2*j]]
                if((3 == event[2*j+1] or event[2*j +1] == 4)) and (i == num_e) or((3 == event[4*j + 1] or event[4*j+1] == 4) and (i==num_f)):
                    speed[event[2*j]] = 0
                    valTurtle[event[2*j]].write("Stuning", font=("Arial", 5, "normal"))
                turRoadLengh[j] += speed[j]
                valTurtle[j].forward(speed[j])
            else:
                flag[j] = flag[j] - 1
                valTurtle[j].goto(-150 + 20 * step, tur_set[j])
                time_r[j] = datetime.datetime.now()
    valTurtle[0].write(currentDT.microsecond - time_r[0].microsecond)
    valTurtle[1].write(currentDT.microsecond - time_r[1].microsecond)
    valTurtle[2].write(currentDT.microsecond - time_r[2].microsecond)
    valTurtle[3].write(currentDT.microsecond - time_r[3].microsecond)


    # if(s1 < 20 * step):
    #     if (E[0] == 0 and step == num_e):
    #         a = 0
    #         valTurtle[0].write("Stuning", font=("Arial", 5, "normal"))
    #     if (E[0] == 2 and step == num_f):
    #         valTurtle[0].right(180)
    #         valTurtle[0].backward(a * 3)
    #         valTurtle[0].right(180)
    #         s1-=3*a
    #     s1 += a
    #     valTurtle[0].forward(a)
    # else:
    #     valTurtle[0].goto(-150 + 20 * step, 100)
    #     S1DT = datetime.datetime.now()
    # if(s2 < 20 * step):
    #     if (E[1] == 0 and step == num_e):
    #         b = 0
    #         valTurtle[1].write("Stuning", font=("Arial", 5, "normal"))
    #     if (E[1] == 2 and step == num_f):
    #         valTurtle[1].right(180)
    #         valTurtle[1].forward(b * 3)
    #         valTurtle[1].right(180)
    #         s2-=3 * b
    #     s2 += b
    #     valTurtle[1].forward(b)
    # else:
    #     valTurtle[1].goto(-150 + 20 * step, 70)
    #     S2DT = datetime.datetime.now()
    # if(s3 < 20 * step):
    #     if (E[2] == 0 and step == num_e):
    #         c = 0
    #         valTurtle[2].write("Stuning", font=("Arial", 5, "normal"))
    #     if (E[2] == 2 and step == num_f):
    #         valTurtle[2].right(180)
    #         valTurtle[2].forward(c * 3)
    #         valTurtle[2].right(180)
    #         s3-=3 * c
    #     s3 += c
    #     valTurtle[2].forward(c)
    #
    # else:
    #     valTurtle[2].goto(-150 + 20 * step, 40)
    #     S3DT = datetime.datetime.now()
    # if(s4 < 20 * step):
    #     if E[3] == 0 and step == num_e:
    #         d = 0
    #         valTurtle[3].write("Stuning", font=("Arial", 5, "normal"))
    #     if E[3] == 2 and step == num_f:
    #         valTurtle[3].right(180)
    #         valTurtle[3].forward(d * 3)
    #         valTurtle[3].right(180)
    #         s4-=3 * d
    #     s4 += d
    #     valTurtle[3].forward(d)
    # else:
    #     valTurtle[3].goto(-150 + 20 * step, 10)
    #     S4DT = datetime.datetime.now()




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
