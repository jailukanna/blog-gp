---
title: turtle example turtle fractal (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtle fractal'


Modules used in program: 
* `import tkinter as tk`
* `import turtle`

## python turtle fractal

Python turtle example: turtle fractal

```python
import turtle
import tkinter as tk

scale = 0.05 # choose 1, 0.05 
power = 1 # nice one: 1, 0.9, 2, 0.8

root = tk.Tk()
canvas = tk.Canvas(master=root, width=2000, height=1500)
canvas.pack()

t = turtle.RawTurtle(canvas)
t.speed(speed=0) # 0 = fastest 

for i in range(10000000):
    t.forward(scale * i)
    t.right(i**power)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
