---
title: turtle example TurtleLSystems (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'TurtleLSystems'

Functions in program: 
* `def drawLevyFractal(i):`
* `def drawFractalPlant2(i):`
* `def drawFractalPlant1(i):`
* `def drawKochSnowflake(i):`
* `def drawMengerSponge(i):`
* `def drawHilbert(i):`
* `def drawHexSerpinsky(i):`
* `def drawDragonCurve(i):`
* `def drawKochSqr(i):        `
* `def drawSerpinsky(i):`
* `def speedUp():`
* `def LSystem(start, constants, rules, iters):`
* `def applyRules(system, rules):`
* `def parseConst(str, stack):`

Modules used in program: 
* `import turtle`

## python TurtleLSystems

Python turtle example: TurtleLSystems

```python
  # -*- coding: utf-8 -*-
"""
Created on Tue Nov  7 14:44:06 2017

@author: Ritoban
"""

from Stack import Stack

serpinskyStart = "F-G-G"
serpinskyConstants = {
  "F": "F10",
  "G": "F10",
  "+": "L120",
  "-": "R120"
}

serpinskyRules = {
  "F": "F-G+F+G-F",
  "G": "GG"
}

kochSqrStart = "F"
kochSqrConstants = {
  "F": "F10",
  "+": "L90",
  "-": "R90"
}

kochSqrRules = {
  "F": "F+F-F-F+F"
}

dragonStart = "F"
dragonConstants = {
  "F": "F2",
  "H": "F2",
  "+": "R90",
  "-": "L90",
}

dragonRules = {
  "F": "F-H",
  "H": "F+H"
}
hexSerpinskyStart = "A"
hexSerpinskyConstants = {
  "A": "F5",
  "B": "F5",
  "+": "L60",
  "-": "R60",
}

hexSerpinskyRules = {
  "A": "B-A-B",
  "B": "A+B+A"
}
hilbertStart = "A"
hilbertConstants = {
  "F": "F5",
  "+": "R90",
  "-": "L90",
  "A": "NA",
  "B": "NA",
}

hilbertRules = {
  "A": "-BF+AFA+FB-",
  "B": "+AF-BFB-FA+"
}
mengerSpongeStart = "F"
mengerSpongeConstants = {
  "F": "F2",
  "G": "F2",
  "+": "R90",
  "-": "L90"
}
mengerSpongeRules = {
    "F": "F+F-F-F-G+F+F+F-F",
    "G": "GGG"
}
kochStart = "F++F++F"
kochConstants = {
  "F": "F2",
  "X": "NA",
  "+": "L60",
  "-": "R60",
}

kochRules = {
  "F": "F-F++F-F",
  "X": "FF"
}
fractalPlant1Start = "FFFFF1"
fractalPlant1Constants = {
  "F": "F10",
  "+": "L25",
  "-": "R25",
  "[": "[",
  "]": "]",
  "1": "NA",
  "2": "NA",
  "3": "NA"
}

fractalPlant1Rules = {
  "1" : "FF[-2][3][+3]",
  "2" : "FF+F-F-F[FFF3][+3]-F-F3",
  "3" : "FF-F+F+F[2][-2]+F+F2"
}
fractalPlant2Start = "X"
fractalPlant2Constants = {
  "F": "F10",
  "+": "L25",
  "-": "R25",
  "[": "[",
  "]": "]",
  "X": "NA"
}
fractalPlant2Rules = {
  "X" : "F[-X][X]F[-X]+FX",
  "F" : "FF",
}
levyStart = "F"
levyConstants = {
  "F": "F5",
  "+": "L45",
  "-": "R45",
  "[": "[",
  "]": "]",
}
levyRules = {
  "F": "+F--F+"
}



from turtle import forward, left, right, speed, penup, pendown, shape, tracer, pos, xcor, ycor, heading, setpos, seth
import turtle


def parseConst(str, stack):
  if(str[0] == "F"):
    forward(int(str[1:]))
  elif(str[0] == "L"):
    left(int(str[1:]))
  elif(str[0] == "R"):
    right(int(str[1:]))
  elif(str[0] == "["):
    stack.push((turtle.xcor(), turtle.ycor(), turtle.heading()))
  elif(str[0] == "]"):
    cur_pos = stack.pop()
    penup()
    setpos(cur_pos[0], cur_pos[1])
    seth(cur_pos[2])
    pendown()
  return stack

def applyRules(system, rules):
  output = ""
  for c in system:
    if c in rules:
      output += rules[c]
    else:
      output += c
  return output

    
def LSystem(start, constants, rules, iters):
    system = start
    stack = Stack()
    for i in range(iters):
      for key, value in rules.items():
        system = applyRules(system, rules)
        
      #print(system)
    for c in system:
        stack = parseConst(constants[c], stack=stack)
def speedUp():
  tracer(8, 25)
def drawSerpinsky(i):
  LSystem(serpinskyStart, serpinskyConstants, serpinskyRules, i)
def drawKochSqr(i):        
  LSystem(kochSqrStart, kochSqrConstants, kochSqrRules, i)
def drawDragonCurve(i):
  LSystem(dragonStart, dragonConstants, dragonRules, i)
def drawHexSerpinsky(i):
  LSystem(hexSerpinskyStart, hexSerpinskyConstants, hexSerpinskyRules, i)
def drawHilbert(i):
  LSystem(hilbertStart, hilbertConstants, hilbertRules, i)
def drawMengerSponge(i):
  LSystem(mengerSpongeStart, mengerSpongeConstants, mengerSpongeRules, i)
def drawKochSnowflake(i):
  LSystem(kochStart, kochConstants, kochRules, i)
def drawFractalPlant1(i):
  LSystem(fractalPlant1Start, fractalPlant1Constants, fractalPlant1Rules, i)
def drawFractalPlant2(i):
  LSystem(fractalPlant2Start, fractalPlant2Constants, fractalPlant2Rules, i)
def drawLevyFractal(i):
  LSystem(levyStart, levyConstants, levyRules, i)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
