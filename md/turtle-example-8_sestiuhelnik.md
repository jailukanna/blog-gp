---
title: turtle example 8 sestiuhelnik (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example '8 sestiuhelnik'


## python 8 sestiuhelnik

Python turtle example: 8 sestiuhelnik

```python
from turtle import forward, left, exitonclick, right

pocet_stran = 6 
vnitrni_uhel = 180 * (1 - 2/pocet_stran) 
delka_strany = 200/pocet_stran

for pocet in range(6):
    for i in range(pocet_stran):
        forward(delka_strany)
        left(180 - vnitrni_uhel)

    forward(delka_strany)
    right(180 - vnitrni_uhel) 
    
exitonclick()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
