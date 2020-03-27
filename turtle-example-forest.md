---
title: turtle example forest (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'forest'

Functions in program: 
* `def main():`
* `def w():`
* `def t():`
* `def s():`
* `def u():`
* `def doit4(level, pen):`
* `def doit3(level, pen):`
* `def doit2(level, pen):`
* `def doit1(level, pen):#create a fun for  position of tree`
* `def start(t,x,y):`
* `def tree(tlist, size, level, widthfactor, branchlists, angledist=10, sizedist=5):`
* `def branch( t, distance, parts, angledist ): #to form branches`
* `def create_tree( branchlist, angledist, sizedist ): #to form the tree`
* `def symRandom(n):`

## python forest

Python turtle example: forest

```python
#!/usr/bin/env python3
"""     turtlegraphics-example-suite:

             tdemo_forest.py

Displays a 'forest' of 3 breadth-first-trees
similar to the one in tree.
For further remarks see tree.py

This example is a 'breadth-first'-rewrite of
a Logo program written by Erich Neuwirth. See
http://homepage.univie.ac.at/erich.neuwirth/
"""
"""
Modified by: Pushpanjali Gupta,M0329018,CGU"""
from turtle import Turtle, colormode, tracer, mainloop, Screen
from random import randrange
from time import clock
wn = Screen()
def symRandom(n):
    return randrange(-n,n+5)

def create_tree( branchlist, angledist, sizedist ): #to form the tree
    return [ (angle+symRandom(angledist),
              sizefactor*1.01**symRandom(sizedist))
                     for angle, sizefactor in branchlist ]

def branch( t, distance, parts, angledist ): #to form branches
    for i in range(parts):
        t.left(symRandom(angledist))#To form inclination
        t.forward( (distance )/parts)#to grow the tree upward

def tree(tlist, size, level, widthfactor, branchlists, angledist=10, sizedist=5):
 
    if level > 0:
        lst = []
        brs = []
        for t, branchlist in list(zip(tlist,branchlists)):
            t.pensize( size * widthfactor )
            t.pencolor( 255 - (180 - 11 * level + symRandom(15)),180 - 11 * level + symRandom(15),127 )
            #To give color to tree
            t.pendown()#Pull the pen down – drawing when moving.
            branch(t, size, level, angledist )
            yield 1
            for angle, sizefactor in branchlist:
                t.left(angle)
                lst.append(t.clone())
                brs.append(create_tree(branchlist, angledist, sizedist))
                t.right(angle)
        for x in tree(lst, size*sizefactor, level, widthfactor, brs,
                      angledist, sizedist):
            yield None


def start(t,x,y):
    colormode(255)
    t.reset()
    t.speed(0)
    t.hideturtle()
    t.left(90)
    t.penup()#Pull the pen up – no drawing when moving
    t.setpos(x,y)
    t.pendown()

def doit1(level, pen):#create a fun for  position of tree
    pen.hideturtle()
    start(pen, 30, 20)
    t = tree( [pen], 80, level, 0.1, [[ (40,0.69), (0,0.65), (-35,0.71) ]] )
    return t

def doit2(level, pen):
    pen.hideturtle()
    start(pen, -135, -130)
    t = tree( [pen], 120, level, 0.1, [[ (45,0.69), (0,0.72), (-45,0.71) ]] )
    return t

def doit3(level, pen):
    pen.hideturtle()
    start(pen, 190, -90)
    t = tree( [pen], 90, level, 0.1, [[ (35,0.7),  (-25,0.65) ]] )
    return t

def doit4(level, pen):
    pen.hideturtle()
    start(pen, 230, 90)
    t = tree( [pen], 100, level, 0.1, [[ (60,0.7),  (-35,0.65) ]] )
    return t


#draw tree
def u():
    wn.bgcolor("LemonChiffon")
    u = doit1(6, Turtle(undobuffersize=2))
    while True:
        done = 0
        for b in u:
            try:
                b.__next__()
            except:
                done += 1
        if done == 1:
            break

def s():
    wn.bgcolor("honeydew2")
    s = doit2(7, Turtle(undobuffersize=5))
    while True:
        done = 0
        for b in s:
            try:
                b.__next__()
            except:
                done += 1
        if done == 1:
            break

def t():
    wn.bgcolor("DarkSalmon")
    t = doit3(5, Turtle(undobuffersize=1))
    while True:
        done = 0
        for b in t:
            try:
                b.__next__()
            except:
                done += 1
        if done == 1:
            break


def w():
    wn.bgcolor("burlywood")
    w = doit4(9, Turtle(undobuffersize=1))
    while True:
        done = 0
        for b in w:
            try:
                b.__next__()
            except:
                done += 1
        if done == 1:
            break

  

# tree generator:
def main():
   
    
    p = Turtle()
    p.ht()
    wn.onkey(u,"u")#using events to show different colors and different tree
    wn.onkey(s,"s")
    wn.onkey(t,"t")
    wn.onkey(w,"w")
    wn.listen()
    tracer(1,10)
    return "Come in my forest~"
    

if __name__ == '__main__':
    msg = main()
    print(msg)
    mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
