---
title: turtle example turtle ellipse (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtle ellipse'


Modules used in program: 
* `import math # kell a matematikai függvények miatt`

## python turtle ellipse

Python turtle example: turtle ellipse

```python
#!/usr/bin/env python3

from turtle import *
import math # kell a matematikai függvények miatt

size_x = 100 # az ellipszis mérete x irányban
size_y = 50 # az ellipszis mérete y irányban

# az ellipszis középpontja legyen tőlünk balra
origo_x = xcor() - size_x;
origo_y = ycor()

step = 5 # lépésköz fokban, minél kisebb ez, annál pontosabb az ellipszis
for i in range(step, 360 + step, step): # körbelépkedjük az origót "step" foknyi lépésközökkel
    goto(origo_x + math.cos(math.radians(i))*size_x, # cos az egységkör hossza adott szögnél
         origo_y + math.sin(math.radians(i))*size_y) # sin az egységkör magassága adott szögnél
         # felszorozzuk mindkét számot a mérettel és eltoljuk az origóba
         # a math.radians szöget radiánná konvertál, ez kell a cos és a sin-nek Pythonban

exitonclick() # kattintásra várunk

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
