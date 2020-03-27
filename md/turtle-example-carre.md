---
title: turtle example carre (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'carre'

Functions in program: 
* `def carre(n):`

## python carre

Python turtle example: carre

```python


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
    carre(80)
    left(15)

for n in range(24):
    carre(10)
    left(15)

done()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
