---
title: turtle example turtle hello world2 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtle hello world2'


Modules used in program: 
* `import re`

## python turtle hello world2

Python turtle example: turtle hello world2

```python
import re
from turtle import *

compass = {'r': 'd', 'd': 'l', 'l': 'u', 'u': 'r'}
angle = lambda a, b, r=0: r if a == b else angle(compass[a], b, r + 90)
letters = {
    'H': 'u50d25r25u25d50',
    'd': 'u25r25u25d50l25R25',
    'e': 'u25r25d12l25D13r25',
    'l': 'u50D50',
    'o': 'u25r25d25l25R25',
    'r': 'u25r25D25',
    'w': 'U25d25r15u13D13r15u25D25',
    ' ': 'R20',
}
cd = 'r'
for c in 'Hello world':
    p = re.split(r'(\d+)', letters[c] + 'R10')
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
