---
title: turtle example prohw (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'prohw'

Functions in program: 
* `def fuzzint(mf, sd):`
* `def random_xy_coord():`
* `def random_length():`
* `def draw_star(x, y, col, side):`

Modules used in program: 
* `import colorsys`
* `import random as r`

## python prohw

Python turtle example: prohw

```python
'''
這邊為簡單的滿天星作圖
內有酸明峰添加創意，見23行可將其消註解
'''
from turtle import *
import random as r
import colorsys
from math import sin, pi

def draw_star(x, y, col, side):
    ds = Turtle()
    ds.hideturtle()
    ds.speed('fastest')
    ds.color(col)
    ds.begin_fill()
    ds.penup()
    ds.goto(x, y)
    ds.pendown()
    for k in range(5):
        ds.forward(side)
        ds.right(144)
        ds.forward(side)
        #ds.left(108)
    ds.end_fill()
def random_length():
    return r.randrange(5, 25)
def random_xy_coord():
    return r.randrange(-400, 400), r.randrange(-300, 300) 

bg = Turtle()
bg.screen.bgcolor('black')

colors = ['red', 'orange', 'magenta', 'green', 'blue', 'yellow', 'white']

stars = 50
for k in range(stars):
    color = r.choice(colors)
    side = random_length()
    x, y  = random_xy_coord()
    draw_star(x, y, color, side)

'''
此為諧波圖結合利薩如曲線作圖
原理跟物理有關，BlurBlur一堆自己去估狗
主要的公式為 x = A * sin (t * f + p) * e-d * t
A = 初始振福
t = 時間
f = 頻率
d = 阻尼係數
p = 初始相位
反正一堆物理概念都不懂，只是需要公式XDDD
'''

bgc = "black"                   # 背景顏色
width, height = 800, 600        # 畫面長寬
npen = 6                        # 鐘擺數量
dd = 0.9999                     # 衰變單位量
dt = 0.018                      # 時間增量
sd = 0.008                      # 傳播頻率
mf = 5                          # 最大頻率
end = 0.25                      # 衰變終點
linewidth = 3                   # 筆寬
hui = dt/(2*pi)                 # 色調增量
xscale, yscale = width / (npen * 1.8), height / (npen * 1.8)
 
def fuzzint(mf, sd):
    return r.gauss(r.randint(1, mf), sd)
 

fxy = [[fuzzint(mf, sd)     for p in range(npen)] for q in range(2)]
pxy = [[r.randint(0, 7)  for p in range(npen)] for q in range(2)]
 
canvas = Screen()
canvas.setup(width, height)
canvas.bgcolor(bgc)
tt = Turtle()
tt.width(linewidth)
tt.speed("fastest")

hue = 0
t = 0.0                         # 時間
dec = 1.0                       # 衰變量
first = True                    # 不重複畫從原點出發的線
tt.penup()
 
while dec > end:
    xy = [sum(sin(t * fxy[q][p] + pxy[q][p])    for p in range(npen))
                                                for q in range(2)]
    A=r.random()
    B=r.random()
    tt.color(colorsys.hsv_to_rgb(hue, A, B))
    tt.goto(xy[0]*xscale*dec, xy[1]*yscale*dec)
    dec *= dd
    hue += hui
    if first:                   
        first = False
        tt.pendown()
    t += dt

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
