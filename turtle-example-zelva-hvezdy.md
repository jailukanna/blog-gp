---
title: turtle example zelva-hvezdy (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'zelva-hvezdy'


Modules used in program: 
* `import random`

## python zelva-hvezdy

Python turtle example: zelva-hvezdy

```python
# tento program vykresluje nahodne barevne a nahodne velke hvezdy
# vyuzivame knihovnu Turtle
# dokumentaci najdete zde:  https://docs.python.org/3/library/turtle.html
# návod na youtube: https://www.youtube.com/watch?v=Aui6I_0LBEI


from turtle import *
import random

# seznam barev
barvy = ['yellow', 'red', 'blue', 'green', 'pink', 'brown', 'black', 'cyan']

# nastavime rychlost vykreslovani co nejvyssi - bez prodlevy
speed(0)

# hvezdy vykreslujeme dal a dal bez konce
while True:

    #zvedneme pero, aby nam zelva nekreslila
    penup()

    # nastavime nahodne x, y souradnice a nahodnou velikost
    x = random.randint(-300,300)
    y = random.randint(-300,300)
    v = random.randint(20,300)

    # vybereme nahdne dve cisla pro barvy
    b1 = random.randint(0, 7)
    b2 = random.randint(0, 7)

    # nahodna cisla barev pouzijeme pro vyber barev ze seznamu
    color( barvy[b1], barvy[b2])

    # skocime na souracnici x,y
    goto( x , y)

    # dame pero dolu a zaciname tedy kreslit
    pendown()

    begin_fill()
    while True:
        # popojdeme o vzdalenost v
        forward(v)

        # otocime se o 170 stupno do leva
        left(170)

        # pokud vzdalenost aktualni pozice neni daleko od puvodne vybranych x,y
        # tak jsme dokreslili a prikazem break vyskocim z cyklu
        if abs(pos() - (x,y)) < 1:
            break
    end_fill()
done()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
