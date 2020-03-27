---
title: turtle example ryTurtleEx02 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'ryTurtleEx02'

Functions in program: 
* `def 主函數():`

## python ryTurtleEx02

Python turtle example: ryTurtleEx02

```python
'''
ryTurtleEx02.py
===============

運用 中文龜模組 turtle_tc.py
------------------------

1. 無名龜 自己畫圖
2. 跟隨滑鼠
3. 聽候鍵盤



by 呂仁園
2014/03/24

'''

from  turtle_tc import *
#
# 程式由此開始
# ------------


def 主函數():

    開幕()

    W= 100

    座標系統(-W, -W, W, W)

    形狀(龜形)

    顏色(紅)

    寫('我是無名龜')

    速度(100)

    def 畫格子線(N=20):

        w= 格子寬= (2*W)/N

        線集=  [ ((-W+w*n,-W),        (-W+w*n, +W))       for n in range(N)]
        線集+= [ ((-W,    -W+w*n),    (+W,     -W+w*n))   for n in range(N)]

        for 線 in 線集:
            起點, 終點= 線
            提筆(); 前往(起點)
            下筆(); 前往(終點)

        提筆();
        筆色(藍);
        前往(0,0); 寫(位置())
        前往(w*N/4,0); 寫(位置())
        前往(0,w*N/4); 寫(位置())

        回家()
        pass

    畫格子線(20)

    多邊形= [(0,0),(50,0),(50,50),(0,50)]

    開始多邊形()

    [前往(x) for x in 多邊形]

    結束多邊形()

    在點擊幕時(前往)

    在按著鍵時(清除)

    聽鍵盤()

    閉幕()

if __name__=='__main__':

    主函數()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
