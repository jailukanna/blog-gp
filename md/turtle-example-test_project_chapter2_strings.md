---
title: turtle example test project chapter2 strings (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'test project chapter2 strings'


## python test project chapter2 strings

Python turtle example: test project chapter2 strings

```python
#########文字列##############
# 文字列同士は足し算可、引き算不可
print("abc"+"def")
a="abcdef"
b="ghijkl"
c=a+b
print("1. "+c)
print("2. "+c[3]) #Javaの配列みたいに文字列の各文字が0-nで対応していて抜き出せる※インデックス
print("3. "+c[-3]) #反対方向から切るにはマイナス
print("4. "+c[3:-3])
print("5. "+c[-3:3])#逆方向に順番に書くのは無理みたいfor文使って結合させる必要がありそう。[n:m]n>m(n,m>0)の必要あり
#表示できなくてもエラーではない？できる範囲で動く模様

print("6. "+ a*3 +b) #掛け算もできる（割り算はできない）
print("7. "+a[2]*3+b[3:3]) #n=mはうまくいかない
# print("8. "+a[0:1,3:5]) #特定の文字だけ抜いて表示することはできない➔a[0:1]+a[3:5}でなければならない




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
