---
title: turtle example ryByteDesign (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'ryByteDesign'

Functions in program: 
* `def 主函數():`

## python ryByteDesign

Python turtle example: ryByteDesign

```python
'''
ryByteDesign.py

呂仁園 中文程式翻譯

2014/05/19

翻譯原則:
     python keyword 不翻
     單字母變數     不翻
'''
#!/usr/bin/env python3
"""      turtle-example-suite:

        tdemo_bytedesign.py

An example adapted from the example-suite
of PythonCard's turtle graphics.

It's based on an article in BYTE magazine
Problem Solving with Logo: Using Turtle
Graphics to Redraw a Design
November 1982, p. 118 - 134

-------------------------------------------

Due to the statement

t.delay(0)

in line 152, which sets the animation delay
to 0, this animation runs in "line per line"
mode as fast as possible.
"""

#from turtle import Turtle, mainloop
from turtle_tc import *

from time import clock as 鐘


class 設計師類(龜類):

    def 設計(我, 家位置, 尺度):

        我.提筆()
        for i in 範圍(5):
            我.前進(64.65 * 尺度)
            我.下筆()
            我.輪子(我.位置(), 尺度)
            我.提筆()
            我.後退(64.65 * 尺度)
            我.右轉(72)

        我.提筆()
        我.前往(家位置)
        我.右轉(36)
        我.前進(24.5 * 尺度)
        我.右轉(198)
        我.下筆()
        我.中角(46 * 尺度, 143.4, 尺度)
        我.取幕().追蹤器(True)

    def 輪子(我, 初始位置, 尺度):

        我.右轉(54)
        for i in 範圍(4):
            我.五角(初始位置, 尺度)
        我.下筆()
        我.左轉(36)
        for i in 範圍(5):
            我.三角(初始位置, 尺度)
        我.左轉(36)
        for i in 範圍(5):
            我.下筆()
            我.右轉(72)
            我.前進(28 * 尺度)
            我.提筆()
            我.後退(28 * 尺度)
        我.左轉(54)
        我.取幕().更新()

    def 三角(我, 初始位置, 尺度):

        舊頭向= 我.頭向()
        我.下筆()
        我.後退(2.5 * 尺度)
        我.三角向右(31.5 * 尺度, 尺度)
        我.提筆()
        我.前往(初始位置)
        我.設頭向(舊頭向)
        我.下筆()
        我.後退(2.5 * 尺度)
        我.三角向左(31.5 * 尺度, 尺度)
        我.提筆()
        我.前往(初始位置)
        我.設頭向(舊頭向)
        我.左轉(72)
        我.取幕().更新()

    def 五角(我, 初始位置, 尺度):

        舊頭向= 我.頭向()
        我.提筆()
        我.前進(29 * 尺度)
        我.下筆()
        for i in 範圍(5):
            我.前進(18 * 尺度)
            我.右轉(72)
        我.五角向右(18 * 尺度, 75, 尺度)

        我.提筆()
        我.前往(初始位置)
        我.設頭向(舊頭向)
        我.前進(29 * 尺度)
        我.下筆()
        for i in 範圍(5):
            我.前進(18 * 尺度)
            我.右轉(72)
        我.五角向左(18 * 尺度, 75, 尺度)

        我.提筆()
        我.前往(初始位置)
        我.設頭向(舊頭向)
        我.左轉(72)
        我.取幕().更新()

    def 五角向左(我, 邊長, 角度, 尺度):

        if 邊長 < (2 * 尺度): return
        我.前進(邊長)
        我.左轉(角度)
        我.五角向左(邊長 - (.38 * 尺度), 角度, 尺度)

    def 五角向右(我, 邊長, 角度, 尺度):

        if 邊長 < (2 * 尺度): return

        我.前進(邊長)
        我.右轉(角度)
        我.五角向右(邊長 - (.38 * 尺度), 角度, 尺度)

    def 三角向右(我, 邊長, 尺度):

        if 邊長 < (4 * 尺度): return

        我.前進(邊長)
        我.右轉(111)
        我.前進(邊長 / 1.78)
        我.右轉(111)
        我.前進(邊長 / 1.3)
        我.右轉(146)
        我.三角向右(邊長 * .75, 尺度)

    def 三角向左(我, 邊長, 尺度):

        if 邊長 < (4 * 尺度): return

        我.前進(邊長)
        我.左轉(111)
        我.前進(邊長 / 1.78)
        我.左轉(111)
        我.前進(邊長 / 1.3)
        我.左轉(146)
        我.三角向左(邊長 * .75, 尺度)

    def 中角(我, 邊長, 角度, 尺度):

        我.前進(邊長); 我.左轉(角度)
        if 邊長 < (7.5 * 尺度):
            return
        我.中角(邊長 - (1.2 * 尺度), 角度, 尺度)

def 主函數():

    設計師= 設計師類()

    設計師.速度(0)
    設計師.藏龜()
    設計師.取幕().延遲(0)
    設計師.取幕().追蹤器(0)

    t0= 鐘()

    設計師.設計(設計師.位置(), 2)

    t1= 鐘()

    return "執行時間: %.2f 秒。" % (t1-t0)

if __name__ == '__main__':

    m= 主函數()
    印(m)
    進入主迴圈()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
