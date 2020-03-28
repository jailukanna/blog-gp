---
title: tkinter example untitled6 main (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'untitled6 main'

Functions in program: 
* `def init_xmdc():`
* `def init_mmc():`
* `def init_mdc():`

Modules used in program: 
* `import op`
* `import extended_euclid`
* `import mdc`
* `import sys`

## python untitled6 main

Python tkinter example: untitled6 main

```python
import sys
import mdc
import extended_euclid
import op



def init_mdc():
    print('calculo do mdc(a,b) ')
    a= int(input('entre com o a   '))
    b= int(input('entre com o b   '))
    minimodiv = mdc.mdc_r(a,b)
    print(minimodiv)

def init_mmc():
    print('calculo do mmc(a,b) ')
    a= int(input('entre com o a   '))
    b= int(input('entre com o b   '))
    maxd = op.mmc(a,b)
    print(maxd)


def init_xmdc():
    print('calculo do mdc(a,b) ')
    a= int(input('entre com o a   '))
    b= int(input('entre com o b   '))
    x,y,d=extended_euclid.xmdc(a,b)
    print("d=ax+by ---> %d = (%d)*(%d) + (%d)*(%d)" % (d,a,x,b,y))
#init_mdc()
#init_xmdc()
init_mmc()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
