---
title: matplotlib example sales visualization (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sales visualization'


Modules used in program: 
* `import matplotlib.dates as mdates`
* `import matplotlib`
* `import matplotlib.pyplot as plt`

## python sales visualization

Python matplotlib example: sales visualization

```python
%matplotlib inline
import matplotlib.pyplot as plt
import matplotlib
import matplotlib.dates as mdates

yoy = df['YOY']
sales_this_month = df['Sales_This_Month']
zero = pd.Series(np.zeros(len(yoy)))
zero.index = yoy.index

# python matplotlib 繪製雙Y軸曲線圖 - https://reurl.cc/y3ZzM
fig, ax1 = plt.subplots(figsize=(15, 8))
ax1.bar(zero.index, sales_this_month, width=15, label='當月營收', color = 'wheat')
ax1.set_ylabel('Sales', fontsize=16)
ax1.get_yaxis().set_major_formatter(
    matplotlib.ticker.FuncFormatter(lambda x, p: format(int(x), ',')))

ax2 = ax1.twinx()
ax2.plot(yoy, label='營收年增百分比')
ax2.plot(zero, '--', color='black')
ax2.set_xlabel('Year', fontsize=12)
ax2.set_ylabel('YOY(%)', fontsize=16)

fig.suptitle('歷史營收/年增率', fontsize=18)
fig.legend()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
