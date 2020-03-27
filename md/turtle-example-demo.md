---
title: turtle example demo (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'demo'

Functions in program: 
* `def pop():`
* `def push():`
* `def G(x):`
* `def F(x, y):`

## python demo

Python turtle example: demo

```python
from lsystems import Rule, expand

@Rule
def F(x, y):
    if x > 0:
        return F(x-1, y+1), G(x)

# rules return a comma-separated list of other rules
@Rule
def G(x):
    if x > 0:
        return push(), G(x-1), pop()
    return

# terminals should return nothing
@Rule
def push():
    pass

@Rule
def pop():
    pass
    
        
if __name__ == "__main__":
    # expand takes an axiom and a depth parameter
    result = expand(F(2,0), 3)

    print(map(lambda x: x.pretty, result))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
