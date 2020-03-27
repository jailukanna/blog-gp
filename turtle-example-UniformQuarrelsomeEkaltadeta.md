---
title: turtle example UniformQuarrelsomeEkaltadeta (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'UniformQuarrelsomeEkaltadeta'


## python UniformQuarrelsomeEkaltadeta

Python turtle example: UniformQuarrelsomeEkaltadeta

```python
from turtle import *

pensize(int(input('How thick do you want your flake? ')))
color = input('Is it humid outside? ')
temp = int(input('What is the temperature outside?'))

if (temp <= 30 and color == 'yes'):
  pencolor('blue')
  for count in range(12):
    forward(200)
    backward(40)
    left(30)
    forward(40)
    backward(40)
    right(60)
    forward(40)
    backward(40)
    left(210)
    forward(40)
    right(30)
    forward(120)
else:
  pencolor('skyblue')
  for count in range(12):
    forward(100)
    backward(20)
    left(30)
    forward(20)
    backward(20)
    right(60)
    forward(20)
    backward(20)
    left(210)
    forward(20)
    right(30)
    forward(60)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
