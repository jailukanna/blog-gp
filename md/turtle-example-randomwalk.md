---
title: turtle example randomwalk (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'randomwalk'

Functions in program: 
* `def walking(colo):`

Modules used in program: 
* `import random`
* `import numpy as np`

## python randomwalk

Python turtle example: randomwalk

```python
import numpy as np
from turtle import *
import random

def walking(colo):
    #setup for turtle
    setpost(0,0)
    shape("turtle")
    color(colo)
    
    #initiate the steps
    steps = 0
    
    while True:
        choice = np.random.randint(5, size=1)
        if choice == 1:
            forward(10)          
            steps+=1
        elif choice == 2:
            right(90)
            forward(10)
            steps+=1
        elif choice == 3:
            left(90)
            forward(10)
            steps+=1
        else:
            backward(10)
            steps+=1
    
    done()
    
    

if __name__ == "__main__":
    walking("mediumturquoise")

    

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
