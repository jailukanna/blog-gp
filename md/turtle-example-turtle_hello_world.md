---
title: turtle example turtle hello world (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtle hello world'


Modules used in program: 
* `import re`

## python turtle hello world

Python turtle example: turtle hello world

```python
import re
from turtle import *

compass = {'r': 'd', 'd': 'l', 'l': 'u', 'u': 'r'}
angle = lambda a, b, r=0: r if a == b else angle(compass[a], b, r + 90)
cmds = (
    'd50u25r25u25d50R35l25u25r25d12l25R35D13u50R10d50R10u25r25d25l25R55'
    'U50d50r15u25d25r15u50R10D25d25r25u25l25R35d25u25r25R10U25d50R35U25l25d25r25u50'
)
p = re.split(r'(\d+)', cmds)
cd = 'r'
for nd, dist in zip(p[::2], p[1::2]):
    right(angle(cd, nd.lower()))
    cd = nd.lower()
    pu() if nd.isupper() else pd()
    fd(int(dist))
ht()
input()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
