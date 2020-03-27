---
title: turtle example test project chapter4 function-2 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'test project chapter4 function-2'

Functions in program: 
* `def bar3(a):`

## python test project chapter4 function-2

Python turtle example: test project chapter4 function-2

```python
# def uranai2(name, hero="hero"):  # 2番目の引数にデフォルト値を設定 キー値はスペース空けない
#     recipes = ['spam', 'egg', 'tomato']
#     print(name, "さんは", sep="", end="")
#     print(recipes[ord(name[0]) % len(recipes)], end="")
#     print("に似て", hero, "です")
#
# uranai2(input("input your name->"))  # ここで関数を使ってもOK
# uranai2('ats', "potato") # デフォルトに新しいものを入れるとちゃんと上書きされる
#
# def uranai3(name, hero="hero"):  # 2番目の引数にデフォルト値を設定 キー値はスペース空けない
#     recipes = ['spam', 'egg', 'tomato']
#     result = name + "さんは"
#     result += recipes[ord(name[0]) % len(recipes)]
#     result += "を食べると" + hero + "です"
#     return result  # ここで今まで足したものを返値に設定することができる
#
# print(uranai3("aaa"))  # uranai3だけ呼んでも意味なし。返値をどう処理するかをセットしておく
#
# def bar():
#     a = 10
#     print(a)
#
# a = 100
# bar()  # bar()を呼んだらaがbar内部の変数に上書きされる
# print(a) # 関数bar()外なのでa=100の定義を利用。むしろbar()内のaはここでは取り出せない
#
# def bar2():
#     a = 10
#     print(a)
#     return a
#
# a = 100
# bar2()     # bar()を呼んだらaがbar内部の変数に上書きされる
# print(a)  # 返したところでどこに帰してるかわからぬから無効

def bar3(a):
    a = a + 5
    print(1000000 + a)
    return a

a = 100
bar3(a)     # bar()を呼んだらaがbar内部の変数に上書きされる
print(bar3(a))  # 返したところでどこに帰してるかわからぬから無効

a = bar3(a) + a
print(a)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
