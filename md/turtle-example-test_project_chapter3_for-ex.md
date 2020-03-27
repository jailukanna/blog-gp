---
title: turtle example test project chapter3 for-ex (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'test project chapter3 for-ex'


## python test project chapter3 for-ex

Python turtle example: test project chapter3 for-ex

```python
for item in range(10):
    print(item, end=" ")  # 終了条件を満たしても次のprintは改行されない

    if item == 9: # ここで空文字を入れて改行させればいいはず
        print("")

for item in range(10):
    print(item, end=" ")  # 終了条件を満たしても次のprintは改行されない
else:  # 上よりも美しい書き方になる
    print("")


for item in range(10, 20, 2):  # (n, m, l)でnからmまでl増えて処理 mちょうどは発生しない
    print(item, end=" ")
else:
    print("")

print("done")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
