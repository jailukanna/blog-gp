---
title: turtle example Hilbert (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Hilbert'

Functions in program: 
* `def main():`
* `def hilbert(n,m):`

## python Hilbert

Python turtle example: Hilbert

```python
from turtle import*

def hilbert(n,m):
    x=20/(2*n+1)
    #结束条件n=1
    if n==1:
        forward(x)
        right(180 * m + 90)
        forward(x)
        right(180 * m + 90)
        forward(x)
    else:
        right(180*m+90)
        #m=1时旋转角度统一变成270，m=0时旋转角度统一变成90
        hilbert(n-1,1-m)
        right(180*m+90)
        forward(x)
        #第二次调用
        hilbert(n-1,m)
        left(180*m+90)
        forward(x)
        left(180*m+90)
        #第三次调用
        hilbert(n-1,m)
        forward(x)
        right(180*m+90)
        #第四次调用
        hilbert(n-1,1-m)
        right(180*m+90)
        
def main():
    #改变n的值可以调整图案的大小
    n=6
    #每5次操作刷新一次，延迟0
    getscreen().tracer(1,0)
    #定义速度
    speed(30)
    #把画笔放到左上角
    up()
    goto(-200,200)
    down()
    hilbert(n,0)
    exitonclick()

main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
