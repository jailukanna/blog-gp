---
title: turtle example test project turtle turtle6 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'test project turtle turtle6'

Functions in program: 
* `def branch(length):`

## python test project turtle turtle6

Python turtle example: test project turtle turtle6

```python
from turtle import *

def branch(length):
    """
    長さを引数として受け取って枝を書く。フラクタルみたいな図になる
    """
    if length < 1:  # lengthが10以下になったら終了
        return
    forward(length)
    left(30)
    branch(length/2)  # 内部でもう一回branchを呼び出す。
    right(60)
    branch(length/2) # 右側の枝を書く
    left(30)
    forward(-length) # 分岐点に戻る

branch(200)

print("done")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
