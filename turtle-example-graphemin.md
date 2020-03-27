---
title: turtle example graphemin (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'graphemin'


Modules used in program: 
* `import random`

## python graphemin

Python turtle example: graphemin

```python
from turtle import *
import random
PEN = Turtle()

class Sommet:
    """docstring for Sommet"""
    x = 0
    y = 0
    clef = 0
    def __init__(self, x,y):
        self.x = x
        self.y = y

    def draw(self):
        PEN.up()
        PEN.goto(self.x,self.y)
        PEN.dot()

class Arc:
    """docstring for Arc"""
    s1 = Sommet(0, 0)
    s2 = Sommet(0, 0)
    poid = 0
    def __init__(self, s1, s2):
        self.s1 = s1
        self.s2 = s2
        self.poid = sqrt(abs(s1.x-s2.x)**2+abs(s1.y-s2.y)**2)

    def draw(self):
        PEN.up()
        PEN.goto(self.s1.x, self.s1.y)
        PEN.down()
        PEN.goto(self.s2.x, self.s2.y)

    def setPoid(self, poid):
        self.poid = poid

    def __str__(self):
        return "Poid :{0}".format(self.poid)

class Graphe:
    liSommet = []
    liArc = []

    def __init__(self, liSommet, liArc):
        self.liSommet = liSommet
        self.liArc = liArc

    def addSommet(self,sommet):
        self.liSommet.append(sommet)

    def addArc(self, arc):
        self.liArc.append(arc)

    def draw(self):
        for sommet in self.liSommet:
            sommet.draw()
        for arc in self.liArc:
            arc.draw()

    def grapheMin(self):
        g = Graphe(self.liSommet[:], [])
        for i in range(len(g.liSommet)):
            self.liSommet[i].clef = i
        self.liArc.sort(cmp=lambda x,y : cmp(x.poid, y.poid))
        for arc in self.liArc :
            if arc.s1.clef != arc.s2.clef:
                g.addArc(arc)
                for sommet in self.liSommet:
                    if sommet.clef == arc.s2.clef and sommet != arc.s2:
                        sommet.clef = arc.s1.clef
                arc.s2.clef = arc.s1.clef
        return g

l1 = []
for i in range(10):
    l1.append(Sommet(random.randrange(-250,250),random.randrange(-250,250)))

l2 = []
for s1 in l1 :
    for s2 in l1:
        if s1 != s2:
            l2.append(Arc(s1,s2))
g = Graphe(l1, l2)
gmin = g.grapheMin()
gmin.draw()

mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
