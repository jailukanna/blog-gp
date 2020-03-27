---
title: turtle example squiggleflower (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'squiggleflower'


## python squiggleflower

Python turtle example: squiggleflower

```python
from turtle import *
from math import sin
from time import sleep
from random import randrange

while True:
    clear()
    tracer(0)
    for xy in ((-640, 270), (0, -270), (640, 270), (-640, -270), (0, 270), (640, -270), (-320, 0), (320, 0)):
        penup()
        goto(xy)
        turnangle = 0
        # turninc = randrange(10, 15)/100
        turninc = randrange(100, 150)/1000
        # turninc = 10/100
        size = 100
        colorshift = randrange(1, 600)/100

        colormode(255)
        bgcolor(230, 230, 230)
        width(randrange(1, 10))
        hideturtle()

        redmult = randrange(3, 10)/10
        greenmult = randrange(3, 10)/10
        bluemult = randrange(3, 10)/10

        if xy[1] is 0:
            length = 3
        else:
            length = 4

        counter = 0

        for x in range(200):
            for y in range(length):
                i = (y * 100 + randrange(1, 100))/100 + 1
                left(turnangle)
                pencolor(abs(int(sin(i*0.5 * redmult + colorshift) * 255)),
                         abs(int(sin(i*0.5 * greenmult + colorshift) * 255)),
                         abs(int(sin(i*0.5 * bluemult + colorshift) * 255)))
                forward(size*(1/(i + 1)) + randrange(-5, 50))

            if counter == 5:
                update()
                counter = 0
            #clear()
            setheading(heading() + randrange(-3, 3))
            penup()
            goto(xy)
            pendown()
            turnangle += turninc - (randrange(1, 100)/1000)
            width(randrange(1, 10))
            counter += 1
    sleep(3)

done()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
