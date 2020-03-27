---
title: turtle example ryMinimal hanoi (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'ryMinimal hanoi'

Functions in program: 
* `def 主函數():`
* `def 玩(x,y):`
* `def 搬多盤(n, 來塔, 去塔, 暫塔):`

## python ryMinimal hanoi

Python turtle example: ryMinimal hanoi

```python
'''
======
河內塔
======

`Tower_of_Hanoi <http://en.wikipedia.org/wiki/Tower_of_Hanoi>`__

'''

#from turtle import *

from turtle_tc import *


層數= 6

class 盤類(龜類):

    def __init__(盤自己, n):

        龜類.__init__(盤自己, shape= 龜形, visible= False)

        盤自己.提筆()
        盤自己.形狀大小(1, n)
        盤自己.填色(n/層數, 0, 1-n/層數)
        盤自己.顯龜()

class 塔類(list):

    def __init__(塔自己, 座標x, 塔色= 紅):

        塔自己.座標x = 座標x
        塔自己.色= 塔色

        顯龜()
        筆色(塔色)
        筆大小(5)
        提筆(); 前往(座標x, -150)
        下筆(); 前往(座標x, +150)
        提筆()
        藏龜()

    def 壓(塔自己, 盤):

        if 盤 is not None:
            盤.設座標x(塔自己.座標x)
            盤.設座標y(-150 + 30*len(塔自己))

            塔自己+= [盤]

    def 彈(塔自己):

        if len(塔自己)>0:
            盤= list.pop(塔自己)
            盤.設座標y(150)
            return 盤

def 搬多盤(n, 來塔, 去塔, 暫塔):

    def 搬1盤(來塔, 去塔):
        盤= 來塔.彈()
        印(' '*n+'%d'%(n))
        去塔.壓(盤)

    if n == 1:
        搬1盤(來塔, 去塔)
    elif n>1:
        搬多盤(n-1, 來塔,  暫塔, 去塔)
        搬1盤(來塔, 去塔)
        搬多盤(n-1, 暫塔, 去塔, 來塔)
    else:
        pass
    return

def 玩(x,y):

    搬多盤(層數, a塔, b塔, c塔)

    寫('點擊 X 結束。')

def 主函數():

    global a塔, b塔, c塔

    開幕()
    標題('河內塔之龜。')

    藏龜(); 提筆(); 前往(0, -225)

    a塔= 塔類(-250,   紅)
    b塔= 塔類(   0,   綠)
    c塔= 塔類(+250,   藍)

    for n in 範圍(層數,0,-1):
        盤= 盤類(n)
        a塔.壓(盤)

    寫('在點擊幕時 玩。')

    在點擊幕時(玩)

    閉幕()

if __name__=="__main__":

    主函數()




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
