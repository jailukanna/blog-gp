---
title: turtle example kytka (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'kytka'

Functions in program: 
* `def dvojlistek(radius, extent, bend=90, stem=30):`
* `def listek_left(radius, extent, bend):`
* `def listek_right(radius, extent, bend):`
* `def kvet(velikost=100, pocet_listku=10):`

## python kytka

Python turtle example: kytka

```python
from turtle import forward, left, right, exitonclick, circle

def kvet(velikost=100, pocet_listku=10):
    for k in range(pocet_listku):
        for i in range(4):
            forward(velikost)
            left(90)
        left(360 / pocet_listku)


def listek_right(radius, extent, bend):
    left(bend)
    circle(radius, extent)
    left(180 - extent)
    circle(radius, extent)
    left(180 - extent)
    right(bend)


def listek_left(radius, extent, bend):
    right(bend)
    right(extent)
    circle(radius, extent)
    left(180 - extent)
    circle(radius, extent)
    left(180 - extent)
    left(extent)
    left(bend)


def dvojlistek(radius, extent, bend=90, stem=30):
    # se stonkama

    # pravy listek
    listek_right(radius, extent, bend)
    forward(stem)

    listek_left(radius, extent, bend)
    forward(stem)


# posunout se vys
left(90)
forward(200)
right(90)

kvet(50, 15)
# otocit dolu
right(90)

# stonek
forward(100)

pocet_listu = 6
vysec_listu = 50
velikost_prvniho_listu = 80
krok_rustu = 20
uhel_uvadnuti = 90
velikost_stonku = 30

for i in range(pocet_listu):
    dvojlistek(velikost_prvniho_listu + krok_rustu * i, vysec_listu, uhel_uvadnuti, velikost_stonku)


exitonclick()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
