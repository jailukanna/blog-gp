---
title: tkinter example untitled6 op (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'untitled6 op'

Functions in program: 
* `def xmdc(a,b):`
* `def mdc_r(a,b):`
* `def mmc(a,b):`
* `def div(a,b):`

## python untitled6 op

Python tkinter example: untitled6 op

```python
def div(a,b):
    if(abs(a)>abs(b)):
        maior=a
        menor=b
    else:
        menor=a
        maior=b
    resto=maior%menor
    div=maior//menor
    return (div,resto)



def mmc(a,b):
    resul= (a*b)//mdc_r(a,b)
    return resul


def mdc_r(a,b):
    if(b==0):
        return a
    else:
        return mdc_r(b, a%b)

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
