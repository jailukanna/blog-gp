---
title: turtle example wombo-combo Exercise3-4 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'wombo-combo Exercise3-4'

Functions in program: 
* `def kresli_ctverec(okolik):`
* `def obsah_elipsy(a,b):`
* `def obvod_obdelnika(sirka,vyska):`

Modules used in program: 
* `import turtle`

## python wombo-combo Exercise3-4

Python turtle example: wombo-combo Exercise3-4

```python
import turtle
from math import pi, sin
turtle.pen(speed=0)
# for j in range(52):
#     turtle.left(5)
#
#     for i in range(4):    # nepracuje se s argumentem i, proste se probehne seznam o i polozkach
#
#         turtle.speed(10)
#         turtle.pencolor("blue")
#         turtle.fillcolor("yellow")
#         turtle.forward(70)
#         turtle.right(90)
#         turtle.shape('turtle')

from math import pi
def obvod_obdelnika(sirka,vyska):
    return 2*(sirka+vyska)

print(obvod_obdelnika(2,3))

def obsah_elipsy(a,b):
    return pi*a*b

print(obsah_elipsy(2,3))





# colors = ["blue", "black", "red", "goldenrod", "green"]
#
# turtle.left(90)
# for i in range(5):
#     turtle.penup()
#     turtle.width(9)
#     if i < 3:
#         turtle.setpos(0 + i * 100, 0)
#     else:
#         turtle.setpos(50 + (i-3) * 100, -75)
#     turtle.pendown()
#     turtle.pencolor(colors[i])
#     turtle.speed(100)
#     turtle.pen(speed=100)
#     for j in range(72):
#         turtle.forward(5)
#         turtle.left(5)
#         turtle.shape('turtle')
# exitonclick()

def kresli_ctverec(okolik):
    for i in range(4):
        turtle.pendown()
        turtle.forward(okolik)
        turtle.right(90)

for i in range(30):
    kresli_ctverec(100)
    turtle.right(30)






turtle.exitonclick()






from random import randrange, choice


print("Pocitac vybral:",choice(["kamen","nuzky","papir"]))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
