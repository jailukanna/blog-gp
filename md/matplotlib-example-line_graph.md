---
title: matplotlib example line graph (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'line graph'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import pandas as pd`
* `import numpy as np`

## python line graph

Python matplotlib example: line graph

```python
# -*- coding: utf-8 -*-
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# 現在時刻から200日分のdatetimeインデックスを作成
x = pd.period_range(pd.datetime.now(), periods=200, freq='d')
x = x.to_timestamp().to_pydatetime()
# ランダム値配列を3列200行分作成し、各列毎に累積和を求める
y = np.random.randn(200, 3).cumsum(0)

plots = plt.plot(x, y)
plt.legend(plots, ('data1', 'data2', 'data3'),             # 3つのプロットラベルの設定
           loc='best',                                     # 線が隠れない位置の指定
           framealpha=0.25,                                # 凡例の透明度
           prop={'size': 'small', 'family': 'monospace'})  # 凡例のfontプロパティ

plt.title('Random Data Graph')  # タイトル名
plt.xlabel('Date')              # 横軸のラベル名
plt.ylabel('Cum. sum')          # 縦軸のラベル名
plt.grid(True)                  # 目盛の表示
plt.tight_layout()              # 全てのプロット要素を図ボックスに収める

# 描画実行
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
