---
title: turtle example squiggleflower2 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'squiggleflower2'


## python squiggleflower2

Python turtle example: squiggleflower2

```python
from turtle import *
from math import sin
from time import sleep
from random import randrange

size = 80

tracer(0)
colormode(255)
bgcolor(230, 230, 230)
hideturtle()

while True:
    turnangle = 0
    # turninc = randrange(10, 15)/100
    turninc = randrange(100, 150)/1000
    # turninc = 10/100

    width(randrange(1, 10))
    colorshift = randrange(1, 600)/100

    redmult = randrange(3, 10)/10
    greenmult = randrange(3, 10)/10
    bluemult = randrange(3, 10)/10

    length = randrange(3, 10)

    counter = 0

    for x in range(300):
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
        # goto(0, 0)
        goto(randrange(-3, 3), randrange(-3, 3))
        pendown()
        turnangle += turninc - (randrange(1, 100)/1000)
        width(randrange(1, 10))
        counter += 1
    sleep(5)
    clear()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
