---
title: turtle example tur (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'tur'


Modules used in program: 
* `import time`

## python tur

Python turtle example: tur

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
from turtle import *
shape("turtle")
import time
color("Blue")
for i in range(4):
    forward(200)
    right(90)
up()
for e in range(2):
    forward(20)
    right(90)
left(90)
down()    
for i in range(4):
    forward(160)
    left(90)    
up()
right(135)
forward(14)
left(135)
a=input("tur sayısı:")

for i in range(4*a):
    forward(180)
    left(90)

time.sleep(5)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
