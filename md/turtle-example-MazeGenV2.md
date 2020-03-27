---
title: turtle example MazeGenV2 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'MazeGenV2'

Functions in program: 
* `def mazeSize():`

## python MazeGenV2

Python turtle example: MazeGenV2

```python
# Random Maze generator, By Peter.
# This maze generator uses the 'Sidewinder' algorithm.
# This version has improved maze generation time (2x quicker!)

def mazeSize():
    x = input("How many squares wide do you want your maze? (Limit is 80) ")
    if x > 80:
        print("That dimension is too big! Try again!")
        mazeSize()
    y = input("How many squares high do you want your maze? (Limit is 40) ") # Height and length of maze in units. NOT pixels!
    if y > 40:
        print("That dimension is too big! Try again!")
        mazeSize()
  # unit = input("In pixels, how big do you want the squares of your maze? ") # The size of the 'units' in pixels.
    if x < y:
        maximum = y
    else:
        maximum = x
    if maximum < 10:
        unit = 50
    elif maximum < 25:
        unit = 30
    else:
        unit = 20
    return(x, y, unit)

x, y, unit = mazeSize()
from turtle import * # The turtle module
from random import * # For making our mazes random

reset() # Clears previous maze.
hideturtle()

setup(width = (x + 4) * unit, height = (y + 4) * unit, startx=None, starty=None)
title("Maze")
delay(0)

vertdata = []
horzdata = [] # Defining arrays for the differnt types of lines.
zerocounter = []
for n in range(0, y): # Makihng the arrays two dimensional.
    vertdata.append([])
    horzdata.append([])
    zerocounter.append([])
count = 0

up()
rt(180)
fd((x / 2) * unit)
rt(90) # Moves the turtle to a suitable starting position
fd((y / 2) * unit)
rt(90)
down()

entrance = randint(0, x - 1)
exit = randint(0, x - 1) # Random positions for entrance and exit.

for n in range(0, x): # Drawing entrance, exit and sides.
    if n == entrance:
        up()
        fd(unit)
        down()
    else:
        fd(unit)
rt(90)
fd(unit * y)
rt(90)
for n in range(0, x):
    if n == exit:
        up()
        fd(unit)
        down()
    else :
        fd(unit)
rt(90)
fd(unit * y)
up()
rt(180)
fd(unit)
lt(90)
fd(unit)
rt(90)

for ny in range(1, y):
    for nx in range(1, x):
        vertline = randint(0, 1)
        vertdata[ny].append(vertline) # adds '1's for every vertical line in the maze

for nx in range(0, x - 1):
    for ny in range(1, y):
        if vertdata[ny][nx] == 1:
            down()
            fd(unit)
            up()
        else:
            fd(unit)
    rt(180)
    fd(unit * (y - 1))
    rt(90)
    fd(unit)
    rt(90)
rt(90)
fd(unit * x)
rt(180)

for ny in range(1, y):
    count = 0
    for nx in range(0, x - 1):
        if nx == 0 and vertdata[ny][nx] == 1:
            count = 0
            zerocounter[ny].append(count)
        elif nx == 0 and vertdata[ny][nx] == 0:
            count += 1
            zerocounter[ny].append(count)
        elif nx > 0 and vertdata[ny][nx] == 1:
            count = 0
            zerocounter[ny].append(count)
        elif nx > 0 and vertdata[ny][nx] == 0:
            count += 1
            zerocounter[ny].append(count)

for ny in range(1, y):
    zerocounter[ny].append(0)

for ny in range(1, y):
    for nx in range(0, x):
        horzdata[ny].append("-")

for ny in range(1, y):
    if zerocounter[ny][0] == 0:
        horzdata[ny].insert(0, 0)
    for nx in range(1, x):
        if zerocounter[ny][nx-1] > 0 and zerocounter[ny][nx] == 0:
            r = randint(0, zerocounter[ny][nx - 1])
            horzdata[ny].insert(nx-r, 0)
        elif zerocounter[ny][nx - 1] == 0 and zerocounter[ny][nx] == 0:
            horzdata[ny].insert(nx, 0)

for ny in range(1, y):
    for nx in range(0, x):
        if horzdata[ny][nx] == "-":
            down()
            fd(unit)
            up()
        else:
            fd(unit)
    rt(90)
    fd(unit)
    rt(90)
    fd(unit * x)
    rt(180)
exitonclick()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
