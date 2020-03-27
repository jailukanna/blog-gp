---
title: turtle example wombo-combo Kresleni (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'wombo-combo Kresleni'


Modules used in program: 
* `import random as rnd`
* `import turtle as tl`

## python wombo-combo Kresleni

Python turtle example: wombo-combo Kresleni

```python
import turtle as tl
import random as rnd
from math import sqrt

# def draw_house(x):
#     for i in range(x):
#         tl.pendown()
#         tl.left(90)
#         tl.forward(50)
#         tl.right(135)
#         tl.forward(sqrt(2)*50)
#         tl.left(135)
#         tl.forward(50)
#         tl.left(90)
#         tl.forward(50)
#         tl.right(135)
#         tl.forward(sqrt(2)*50/2)
#         tl.right(90)
#         tl.forward(sqrt(2)*50/2)
#         tl.right(90)
#         tl.forward(sqrt(2)*50)
#         tl.left(135)
#         tl.forward(50)
#         tl.forward(20)
#     tl.exitonclick()
#
# tl.penup()
# tl.setpos(-300,0)
# draw_house(5)


# def draw_ornament(x,y):
#     tl.pendown()
#     for i in range(x):
#         tl.forward((y+i*10))
#         tl.left(90)
#
# draw_ornament(20,20)

# def draw_star(x):
#
#     while x > 0:
#         tl.pendown()
#         set_size = rnd.randrange(2, 30)
#         tl.left(60)
#         for i in range(6):
#             tl.forward(set_size/2)
#             tl.right(60)
#             tl.forward(set_size/2)
#             tl.left(120)
#         tl.
#         tl.penup()
#         tl.setpos(rnd.randrange(0,200),rnd.randrange(0,200))
#         x -= 1
#
# tl.pen(speed=10)
# draw_star(50)

first = int(input("Zadej prvni cislo: "))
second = int(input("Zadej druhe cislo: "))
ope = input("Zadej operaci: ")
if ope == "+":
    res = first+second
elif ope == "-":
    res = first-second
elif ope == "*":
    res = first*second
elif ope == "/":
    res = first/second

print("{} {} {} = {}".format(first,ope,second,res))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
