---
title: turtle example turtle font (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtle font'

Functions in program: 
* `def lower_i():`
* `def upper_h():`
* `def move_to_next_letter():`

## python turtle font

Python turtle example: turtle font

```python
'''
1. 把以下程式碼存成檔名 turtle_font.py

2. 把它含你的程式放在同一個 檔案夾

3. 再重新執行你的程式看看
'''

# turtle_font.py
# This is an example of turtle font with very few letters defined!

from turtle import *

# We require a move to start of next letter function
def move_to_next_letter():
    penup()
    right(90)
    forward(40)
    left(90)
    pendown()

# draw the letter H
def upper_h():
    left(90)
    forward(100)
    back(50)
    right(90)
    forward(40)
    left(90)
    forward(50)
    back(100)
    move_to_next_letter()

# Draw the letter i
def lower_i():
    forward(50)
    penup()
    forward(25)
    stamp()
    back(75)
    move_to_next_letter()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
