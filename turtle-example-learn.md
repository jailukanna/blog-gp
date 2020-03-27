---
title: turtle example learn (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'learn'

Functions in program: 
* `def yildiz(buyuklugu):`
* `def sag (aci):`
* `def hiz (hiz):`
* `def yukari(deger):`
* `def kalemRengi(renk):`
* `def yer(x,y):`
* `def kalemBuyuklugu(buyukluk):`
* `def ileri(deger):`
* `def daire(alan):`

Modules used in program: 
* `import math`

## python learn

Python turtle example: learn

```python
#coding: cp857

from turtle import *
import math

kap = Turtle()

def daire(alan):
    kap.circle(alan)
def ileri(deger):
    kap.forward(deger)
def kalemBuyuklugu(buyukluk):
    kap.pensize(buyukluk)
def yer(x,y):
    kap.setposition(x,y)
def kalemRengi(renk):
    kap.color(renk)
def yukari(deger):
    kap.up(deger)
def hiz (hiz):
    kap.speed(hiz)
def sag (aci):
    kap.right(aci)

kalemBuyuklugu(3)

def yildiz(buyuklugu):
    for i in range(5):
        ileri(buyuklugu)
        sag(144)
    
yildiz(50)
kap.up()
ileri(100)
kap.down()

yildiz(100)

kap.up()
ileri(150)
kap.down()
yildiz(150)

mainloop()





```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
