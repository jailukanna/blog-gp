---
title: turtle example l-system (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'l-system'


## python l-system

Python turtle example: l-system

```python
from turtle import *

bgcolor('black')
color('red')
# (A → B−A−B), (B → A+B+A)
pattern = 'A+B+A-B-A-B-A+B+A'
angle = 60

for x in pattern:
    print(x)
    if x == 'A' or x == 'B':
        forward(100)
    elif x == '-':
        right(angle)
    elif x == '+':
        left(angle)

done()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
