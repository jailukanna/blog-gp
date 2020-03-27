---
title: turtle example BeispielTurtle (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'BeispielTurtle'

Functions in program: 
* `def main():`
* `def figure(s,n):`
* `def npolygon(side, n):`
* `def quadrat(s):`

## python BeispielTurtle

Python turtle example: BeispielTurtle

```python
#!/usr/bin/python3
# Autor: Gregor Luedi

from turtle import *


def quadrat(s):
    for i in range(4):
        fd(s)
        lt(90)


def npolygon(side, n):
    for i in range(n):
        fd(side)
        lt(360/n)

def figure(s,n):
    for i in range(n):
        quadrat(s)
        fd(s)
        rt(360/n)

def main():
    n = int(input('How many sides? '))
    figure(100,n)
    print('Work is done, the turtle is tired')


if __name__ == '__main__': main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
