---
title: turtle example test project chapter3 if-ex (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'test project chapter3 if-ex'


## python test project chapter3 if-ex

Python turtle example: test project chapter3 if-ex

```python
year = 1912
if year == 1868:
    print("明治元年")
else:
    if year == 1912:
        print("大正元年")
    else:
        print("明治", year-1867, "年")
# else if を重ねる時に内部に作っていくと見にくい

if year == 1868:
    print("明治元年")
elif year == 1912:      # elifを使って書くことで同じレベルで書ける
    print("大正元年")
else:
    print("明治", year - 1867, "年")

# ##########  西暦を和暦に変換する ############### #
seireki = int(input("西暦->"))

if seireki == 1868:
    print("明治元年")
elif seireki < 1912:  # 明治か
    print("明治", seireki-1867, "年")  # 上から処理していくので1868年は処理されない
elif seireki == 1912:
    print("大正元年")
elif seireki < 1926: # 大正か
    print("大正",seireki-1911,"年")
elif seireki == 1926:
    print("昭和元年")
elif seireki < 1989:
    print("昭和", seireki-1925, "年")
elif seireki == 1989:
    print("平成元年")
else:
    print("平成",seireki-1988,"年")

# input()　()内の文字を表示して、同じ行に入力したものを読める
# int() カッコ内を整数に変換。文字列を入れたらエラー
# float()カッコ内を浮動小数に変換
# str()カッコ内を文字列に変換




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
