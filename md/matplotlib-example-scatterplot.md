---
title: matplotlib example scatterplot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'scatterplot'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python scatterplot

Python matplotlib example: scatterplot

```python
# -*- coding: utf-8 -*-
import numpy as np
import matplotlib.pyplot as plt


num_points = 100
gradient = 0.7
x = np.array(range(num_points))                     # x: 0 ~ 100
y = np.random.randn(num_points) * 10 + x*gradient   # y: 正規分布の乱数を0~10の間に拡大して、xに依存する値を加算

fig, ax = plt.subplots(figsize=(8, 4))
ax.scatter(x, y)            # 散布図のプロット

m, c = np.polyfit(x, y ,1)  # 直線の傾き、定数項の取得
ax.plot(x, m　*　x + c)        # 回帰直線のプロット
fig.suptitle('Scatterplot With Regression-line')

plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
