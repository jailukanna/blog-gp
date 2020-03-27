---
title: turtle example test project chapter5 add tax (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'test project chapter5 add tax'

Functions in program: 
* `def add_tax1(astring1):  # 似た名前の変数を付けるとアラートしてくる`
* `def add_tax(astring):`

## python test project chapter5 add tax

Python turtle example: test project chapter5 add tax

```python
def add_tax(astring):
    items = astring.split()  # スペースで区切ってリスト化
    price = int(items[1]) * 1.1  # 左から2番目を数字部分に変換し、1.1倍
    items[1] = str(int(price))  # priceを文字列に変換してリストに戻す
    return " ".join(items)

print(add_tax("Goods 1000 2012/05/27"))


def add_tax1(astring1):  # 似た名前の変数を付けるとアラートしてくる
    items = astring1.split()  # スペースで区切ってリスト化
    price = int(items[1]) * 1.1  # 左から2番目を数字部分に変換し、1.1倍
    items[1] = str(price)  # 文字列に変換しないとjoinできない 間にintを挟まないと小数点の後遺症がでる
    return " ".join(items)

print(add_tax1("Goods 1000 2012/05/27"))  # Gaussianと同じで最後に空行が必要

s = "0abcdefghijklmnopqrstuvwxyz"

print(s.find("0"))  # "z"がある位置を返す
print(s.index("0"))  # 同上

print(s.endswith("y", 0, 26))  # 検索した文字列で元の文字列が終わった時にTrueを返す
# 終了範囲まで調べているので厳密には上の表記はおかしい objectがある時点でお察しの雰囲気もある
if s.startswith("a", 1):  # 上記の頭版
    print("true")





```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
