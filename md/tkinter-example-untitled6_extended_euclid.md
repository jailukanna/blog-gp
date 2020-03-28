---
title: tkinter example untitled6 extended euclid (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'untitled6 extended euclid'

Functions in program: 
* `def xmdc(a,b):`

Modules used in program: 
* `import sys`

## python untitled6 extended euclid

Python tkinter example: untitled6 extended euclid

```python

#d=mdc(a,b) = ax+by

import sys

def xmdc(a,b):
    if b==0:
        return [1,0,a]
    else:
        x,y,d=xmdc(b, a%b)

        return [y,x-(a//b)*y,d]


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
