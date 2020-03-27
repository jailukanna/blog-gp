---
title: turtle example Tree (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Tree'

Functions in program: 
* `def main():`
* `def maketree():`
* `def tree(plist, l, a, f):`

## python Tree

Python turtle example: Tree

```python
#!/usr/bin/env python3
"""
M0329007 邱思綺
"""
from turtle import Turtle, mainloop
from time import clock

def tree(plist, l, a, f):
    """ plist is list of pens
    l is length of branch
    a is half of the angle between 2 branches
    f is factor by which branch is shortened
    from level to level."""
    if l > 3:
        lst = []
        for p in plist:
            p.forward(l)
            q = p.clone()
            p.left(a)
            q.right(a)
            lst.append(p)
            lst.append(q)
        for x in tree(lst, l*f, a, f):
            yield None

def maketree():
    p = Turtle()
    p.speed(0)
    p.getscreen().tracer(30,0)
    p.left(90)
    p.penup()
    p.forward(-210)
    p.pendown()
    t = tree([p], 200, 65, 0.6375)
    for x in t:
        pass
    print(len(p.getscreen().turtles()))



def main():
    a=clock()
    maketree()
    b=clock()
    return "done: %.2f sec." % (b-a)

if __name__ == "__main__":
    msg = main()
    print(msg)
    mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
