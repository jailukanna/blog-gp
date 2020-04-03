---
title: matplotlib example line graph01 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'line graph01'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python line graph01

Python matplotlib example: line graph01

```python
# -*- coding: utf-8 -*-
import numpy as np
import matplotlib.pyplot as plt

labels = ["data1", "data2", "data3", "data4"]
data = [4, 6, 10, 2]

# 表示位置設定
x_width = 0.5
x_loc = np.array(range(len(data))) + x_width

plt.title("Bar Graph")
plt.bar(x_loc, data, width=x_width)     # 棒グラフの設定
plt.xticks(x_loc, labels)               # x軸にラベル設定

# 描画実行
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
