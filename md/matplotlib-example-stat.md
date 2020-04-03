---
title: matplotlib example stat (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'stat'


Modules used in program: 
* `import seaborn as sns`
* `import matplotlib.pyplot as plt`

## python stat

Python matplotlib example: stat

```python
# 直方图，normed表示是否转为密度
import matplotlib.pyplot as plt
plt.hist(df.UseDays, bins=50, color='steelblue', normed=True)

# 直方图及拟合概率密度曲线
import seaborn as sns
sns.distplot(df.UseDays, rug=True, hist=True)

# 使用指定曲线拟合
from scipy import stats
sns.distplot(df.UseDays,kde=False, fit=stats.expon)

# 拟合概率密度曲线
# kernel包括gaussian、tophat、epanechnikov、exponential、linear、cosine
# http://www.dataivy.cn/blog/%E6%A0%B8%E5%AF%86%E5%BA%A6%E4%BC%B0%E8%AE%A1kernel-density-estimation_kde/
from sklearn.neighbors import KernelDensity
X = df.UseDays[:, np.newaxis] #待拟合数据
X_plot = np.linspace(0, 1000, 500)[:, np.newaxis]
kde = KernelDensity(kernel='gaussian', bandwidth=30).fit(X)
log_dens = kde.score_samples(X_plot)
plt.plot(X_plot, np.exp(log_dens))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
