---
title: turtle example chanoi (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'chanoi'

Functions in program: 
* `def main():`
* `def play():`
* `def hanoi(n, nerden, neyle, nereye):`

## python chanoi

Python turtle example: chanoi

```python
#!/usr/bin/python
#-*-coding:utf-8-*-

from turtle import *

class Disc(Turtle):
    def __init__(self, n):
        Turtle.__init__(self, shape="square", visible=False)
        self.pu()
        self.shapesize(1.5, n*1.5, 2) # kare-->dikdörtgen
        self.fillcolor(n/6., 0, 1-n/6.)
        self.st()

class Tower(list):
    def __init__(self, x):
        "boş bir kule yarat."
        self.x = x
    def push(self, d):
        d.setx(self.x)
        d.sety(-150+34*len(self))
        self.append(d)
    def pop(self):
        d = list.pop(self)
        d.sety(150)
        return d

def hanoi(n, nerden, neyle, nereye):
    if n > 0:
        hanoi(n-1, nerden, nereye, neyle)
        nereye.push(nerden.pop())
        hanoi(n-1, neyle, nerden, nereye)

def play():
    onkey(None,"space")
    clear()
    hanoi(6, t1, t2, t3)
    write("çıkmak için DUR tuşuna bas",
          align="center", font=("Courier", 16, "bold"))

def main():
    global t1, t2, t3
    ht(); penup(); goto(0, -225)   # writer turtle
    t1 = Tower(-250)
    t2 = Tower(0)
    t3 = Tower(250)
    # 6 diskli kule yap
    for i in range(3,0,-1):
        t1.push(Disc(i))
    write("başlamak için space tuşuna bas",
          align="center", font=("Courier", 16, "bold"))
    onkey(play, "space")
    listen()
    return "PROGRAM YÜRÜTÜLÜYOR"

if __name__=="__main__":
    msg = main()
    print(msg)
    mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
