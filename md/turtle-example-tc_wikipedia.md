---
title: turtle example tc wikipedia (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'tc wikipedia'

Functions in program: 
* `def 主函數():`
* `def mn_eck(p, ne,sz):`

## python tc wikipedia

Python turtle example: tc wikipedia

```python
'''龜例如套房：

          tdemo_wikipedia3.py

本實施例是
靈感在龜維基百科文章
圖形。 （參見例如wikipedia1的網址）

首先，我們創建（ NE- 1 ） （即在這35
例如）複製我們的第一個烏龜P的。
然後，我們讓他們在履行自己的步驟
平行。

其次是一個完整的撤消（ ） 。=== 以上由 Google 翻譯，請協助改善 ===
'''
from turtle_tc import *; from turtle_tc import 幕類, 龜類, 主迴圈
from time import clock, sleep

def mn_eck(p, ne,sz):
    龜列表 = [p]
    # 創建NE -1的其他海龜
    for i in 範圍(1,ne):
        q = p.複製()
        q.右轉(360.0/ne)
        龜列表.append(q)
        p = q
    for i in 範圍(ne):
        c = abs(ne/2.0-i)/(ne*.7)
        # 讓那些NE海龜做了一步
        # 並行：
        for t in 龜列表:
            t.右轉(360./ne)
            t.筆色(1-c,0,c)
            t.前進(sz)

def 主函數():
    s = 幕類()
    s.背景色(黑)
    p=龜類()
    p.速度(0)
    p.藏龜()
    p.筆色(紅)
    p.筆粗(3)

    s.追蹤(36,0)

    at = clock()
    mn_eck(p, 36, 19)
    et = clock()
    z1 = et-at

    sleep(1)

    at = clock()
    while any([t.回復暫存區的個數() for t in s.龜群()]):
        for t in s.龜群():
            t.回復()
    et = clock()
    return "runtime: %.3f sec" % (z1+et-at)


if __name__ == '__main__':
    訊息 = 主函數()
    印(訊息)
    主迴圈()

# Above: "tc_wikipedia.py", by Renyuan Lyu (呂仁園), 2015-01-27
# Original: "wikipedia.py", by Gregor Lingl. 


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
