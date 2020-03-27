---
title: turtle example ukoly zelva tvary (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'ukoly zelva tvary'


## python ukoly zelva tvary

Python turtle example: ukoly zelva tvary

```python

from turtle import forward, left, speed, exitonclick 

speed('fastest')

# kolecko
for krok in range(360):
    forward(1)
    left(1)

forward(200)

# spirala
for i in range(360):
    forward(i/15)
    left(15)

forward(200)

# listek = 2 x ctvrtkolecko
for cara in range(2):
    for krok in range(90):
        forward(1)
        left(1)
    left(90)

exitonclick()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
