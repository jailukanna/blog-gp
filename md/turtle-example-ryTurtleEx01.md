---
title: turtle example ryTurtleEx01 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'ryTurtleEx01'

Functions in program: 
* `def 主函數():`

## python ryTurtleEx01

Python turtle example: ryTurtleEx01

```python
'''
ryTurtleEx01.py
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

    W= 幕寬= 100
    設座標系統(0,0,W,W)
    背景色 (灰)
    標題('小烏龜會畫圖。')

    生一隻龜()

    形狀(龜形)
    顏色(紅)

    速度(0)       # 0 是最快的; 1 ~ 10 最慢 ~ 很快
    提筆()
    回家()

    def 畫格子點():

        N= 10
        for x in 範圍(N+1):
            提筆(); 前往(W/N*x, 0)
            下筆(); 前往(W/N*x, W)

        for y in 範圍(N+1):
            提筆(); 前往(0, W/N*y)
            下筆(); 前往(W, W/N*y)

    畫格子點()

    提筆();前往(W/2, W/2)

    def 畫星狀圖():

        下筆();
        for i in 範圍(100):
            前進(W/4)
            左轉(170)
            顏色(隨機數(),隨機數(),隨機數())

    畫星狀圖()

    def 畫多個星狀圖(K=2):

        for i in 範圍(K):
            前往(隨機數()*W,隨機數()*W)

            蓋章()
            寫('(%.1f, %.1f)'%(座標x(), 座標y()))

            畫星狀圖()

            睡(1)

    畫多個星狀圖(K=2)

    現在時間= 看時間()
    寫('現在時間= %s'%現在時間)

    回家()

    睡(1)


    def 叫烏龜前往且算距離(x,y):

        前一點座標x, 前一點座標y= 座標x(), 座標y()

        前往(x,y)
        到前一點的距離= 距離(前一點座標x, 前一點座標y)

        寫('(%.1f, %.1f) 距離= %.1f'%(座標x(), 座標y(), 到前一點的距離 ))


    在滑鼠鍵點擊幕時(叫烏龜前往且算距離)

    #在鍵盤鍵壓下時(清除幕)
    在按鍵時(清除幕,空白鍵)


    聽鍵盤()

    筆色(藍)
    寫('''
    在滑鼠鍵點擊幕時(叫烏龜前往)
    在鍵盤鍵壓下時(清除幕)
    點擊 X 結束。
    ''')

    閉幕()

if __name__=='__main__':
    主函數()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
