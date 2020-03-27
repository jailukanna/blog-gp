---
title: turtle example test project chapter3 ronrienzanshi (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'test project chapter3 ronrienzanshi'


## python test project chapter3 ronrienzanshi

Python turtle example: test project chapter3 ronrienzanshi

```python

age = 0

if age <= 18 or age >= 65 and age != 0:
    print("true")
else:
    print("false")

# andの優先順位の方がorより高いので
# true or false and false -> true or false -> trueと判断される
# なので先にorを倒す

if (age <= 18 or age >= 65) and age != 0:
    print("true")
else:
    print("false")

# ############### 比較 ############### #
print("-------------------comparative-------------------")
s = "This is It"
b = str(s == "This is It")
print(b)

b = str(s == "This is it")
print(b,"大小も大事")

a = [1, 2, 3, 4]
b = bool(a == [1, 2, 3, 4])
print(b,"リストの比較もできる",end="   ",sep="<--")  # end,sepのオブジェクトでprintのふるまいを決められる
b = bool(a == [1, 2, 3])
print(b,"リストも完全一致の必要あり")

# ################  inで調べる  ################# #
print("-------------------comparative-------------------")
st = "this is python"
if "python" in st:  # inを使って部分一致してたらtrue
    print(("python is in st"))

l = [1, 2, 3, 4]
if 1 in l:
    print("true")
else:
    print("false")  # リストも調べられる。

numone = 1
strone = "1"
if numone == strone: # 型が一致しないと比較がうまくいかない
    print(1)
elif numone == int(strone):
    print(2)
else:
    print(3)

if [1, 2, 3] == {1, 2, 3}:
    print("OK")
else:
    print("NG")  # リストとタプルは比較できないから要素を抽出するかtuple(),list()に変更して比較

# リスト内のリストの変更はいかにして行うのか
if [[1, 2, 3], 2, 3] == [list({1, 2, 3}), 2, 3]:
    print("OK")
else:
    print("NG")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
