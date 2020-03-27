---
title: turtle example carre2 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'carre2'

Functions in program: 
* `def carre(n):`

## python carre2

Python turtle example: carre2

```python
angle1 = float(input("Donnez un angle"))
mesure = float(input("Donnez une mesure"))

from turtle import *

def carre(n):
    forward(n)
    left(90)
    forward(n)
    left(90)
    forward(n)
    left(90)
    forward(n)
    left(90)

for n in range(24):
    carre(mesure)
    left(angle1)

for n in range(24):
    carre(mesure)
    left(angle1)

done()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
