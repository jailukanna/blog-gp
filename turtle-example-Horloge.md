---
title: turtle example Horloge (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Horloge'

Functions in program: 
* `def main():`

## python Horloge

Python turtle example: Horloge

```python
from turtle import *
from time import *

class aiguille:
    def __init__(self, quelle, epaisseur, taille):
        self.tortue = Pen()
        self.taille = taille
        self.epaisseur = epaisseur
        self.valeur_temps = {'h':3, 'm':4, 's':5}[quelle]
        self.denominateur = {'h':12, 'm':60, 's':60}[quelle]
        
    def tracer(self):
        self.tortue.reset()
        self.tortue.speed('fastest')
        self.tortue.pensize(self.epaisseur)
        self.tortue.setheading(450-(360*localtime()[self.valeur_temps]/self.denominateur))
        self.tortue.forward(self.taille)

def main():
    h = aiguille('h', 4, 60)
    m = aiguille('m', 2, 90)
    s = aiguille('s', 1, 130)
    while True:
        h.tracer()
        m.tracer()
        s.tracer()
        sleep(1)

main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
