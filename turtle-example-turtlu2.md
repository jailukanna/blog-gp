---
title: turtle example turtlu2 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtlu2'


## python turtlu2

Python turtle example: turtlu2

```python
from turtle import *

shape('turtle')

color('yellow')

begin_fill()

pensize(4)
speed(2)

"""
for color in ["black","red","blue","green"]:
    pencolor(color)
    forward(50)
    right(90)

end_fill()##icini yellow yapar

clear()
"""

penup() #kalemi kaldırır

goto(0,0)

pendown() #kalemi tekrar çizm yerine kor

fillcolor("red") #çizilecek nesnenin içini doldurur



for i in range(4):
    forward(i*20)
    left(90)
    forward(i*20)
    left(90)
    forward(i*20)
    left(90)

done()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
