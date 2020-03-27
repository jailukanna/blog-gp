---
title: turtle example test project chapter2 datatype (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'test project chapter2 datatype'


## python test project chapter2 datatype

Python turtle example: test project chapter2 datatype

```python
################データ型##############
#①リスト---配列みたいなもの
a=[0,1,2,3,4,5,6,7,8,9,]#index
print(a[2])
print(a)
a[6]=66#上書きできる
a[0]=a[0]+3#足し算もできる
print(a)

b=["apple","byte","c++","dim"]
print(b)
b[2]=4
print(b)#リストは文字列も数値も区別しない

#リストの中にリストを入れることも可能（2次元的な拡張だけど、大きさにルールはない）
c=[[1,2,3,4],[2,4,6],[3,6,9,12,15]]
print(c[1][2])
print(c[2])

d=[[[1,2,3,],4,5],6,7,8]
print(d[0][0][0])#n次元に広げられる。とても見にくい

a.append([1,2])#aにappend(x)でxを最後に加えることができる
print(a)
del a[3]
print(a)
print(a[3]) # 消すことも可能、消すとインデックスも付け直される

# ②タプル---変えられない配列
e=(0,1,2,3,4,5) # ()で作るリスト※呼び出す時は[]
print(e)
# e[2]=5  #書き換えることができない
f = ([0.0, 0.1, 0.2, 0.3], 1, 2, 3)
print(f)
f[0][1] = 1
print(f) # タプルの中のリストなら変えることができる。変えたくなければさらにタプルの必要がある

# ③辞書---キーを使って引っ張り出す
dic = {"red": 123, "green": 3, 22: 2}
print(dic["red"])
print(dic)
dic["red"] = 1  # 上書きも可能。データ型もなんでもござれ
print(dic[22])
dic["blue"] = 1234567
print(dic)
del dic["green"]
print(dic) # 加えるのも消すのも可能。内部で順番は関係ない（呼び出す時にキー値を使うから）
#  print("icon") print("egypt")#行内で2回printができない？


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
