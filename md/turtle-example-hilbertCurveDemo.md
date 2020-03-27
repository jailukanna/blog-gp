---
title: turtle example hilbertCurveDemo (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'hilbertCurveDemo'

Functions in program: 
* `def main():`
* `def B(s):`
* `def A(s):`
* `def forward(x):`
* `def right(d):`
* `def left(d):`

## python hilbertCurveDemo

Python turtle example: hilbertCurveDemo

```python
from lsystems import Rule, expand

# terminals should return nothing
@Rule
def left(d):
    pass

@Rule
def right(d):
    pass

@Rule
def forward(x):
    pass

@Rule
def A(s):
    return [
        left(90),
        B(s),
        forward(s),
        right(90),
        A(s),
        forward(s),
        A(s),
        right(90),
        forward(s),
        B(s),
        left(90)]

@Rule
def B(s):
    return [
        right(90),
        A(s),
        forward(s),
        left(90),
        B(s),
        forward(s),
        B(s),
        left(90),
        forward(s),
        A(s),
        right(90)]


# couldn't resist the pun, sorry
turtleShell = """
from turtle import *

# general setup

speed(0)
penup()
setposition(-window_width()/2 + 15, -window_height()/2 + 15)
pendown()
color('green')

%s

# waits for user action
done() 
"""
    
def main():
    # expand from Axiom A 5 times
    result = expand(A(25), 5)

    # TODO: these things probably belong in the lsystems package
    # convert output to functional string syntax
    result = map(lambda x: x.pretty, result)    
    # filter out non-terminals and put each command on its own line
    commands = "\n".join(filter(lambda x: x[0] not in ['A', 'B'], result))

    # adds the generated commands to the file
    turtleProgram = turtleShell % commands

    exec(turtleProgram)

    # another option is to write turtleProgram to a file and run it separately
    #with open('outputProgram.py', 'w') as f:
    #    f.write(turtleProgram)


if __name__ == "__main__":
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
