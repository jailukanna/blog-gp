---
title: turtle example gpug (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'gpug'

Functions in program: 
* `def do( (command, parameter)):`

## python gpug

Python turtle example: gpug

```python
# -*- coding: utf-8 -*-
"""
Created on Fri May 16 09:04:31 2014

@author: tobie nortje @tooblippe

GPUG LOGO - just for fun
"""

from turtle import Turtle

t = Turtle()

#shorter functions names
l = t.left
r = t.right
f = t.fd
b = t.back
pu = t.pu
pd = t. pd
pf = t.pd
goto = t.goto

#scaling
scale = 3.0 # scaling factor - smaller makes GPUG bigger
line_color = 'black'
line_weight = '2'

t.pensize(line_weight)


g = [
    (l, 90),
    (f, 100),
    (l, 90),
    (f, 50),
    (l, 90),
    (f, 50),
    (pu, None),
    (f, 50),
    (l, 90),
    (f, 50),
    (r, 180),
    (pd, None),
    (f, 100),
    (r, 90),
    (f, 200),
    (r, 90),
    (f, 100)
   ]

p = [ (pu, None),
     (f, 10),
     (pd, None),
     (f, 100),
     (r, 90),
     (f, 100),
     (r, 90),
     (f, 100),
     (r, 90),
     (f, 100),
     (b, 200)
     ]

u = [ (r, 90),
      (pu, None),
      (f, 120),
      (pd, None),
      (l, 90),
      (f, 200),
      (b, 200),
      (r, 90),
      (f, 100),
      (l, 90),
      (f, 200),
      (b, 200),
      (r,90),
      (pu, None),
      (f, 120),
      (pd, None)
      ]

#lets draw GPUG

gpug_logo = [ g, p, u, g]

def do( (command, parameter)):
    if command == f or command == b:
        parameter = parameter / scale
        
    if command == goto:
        pu()
        command( parameter)
        pd()
    elif command == pu: pu()
    elif command == pd: pd()
    else:
        command(parameter)
        
for letter in gpug_logo : 
    for command in letter:
        do(command)

t.hideturtle()
print("press CTR-C to exit")
while True:
    pass

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
