---
title: matplotlib example general (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'general'


Modules used in program: 
* `import matplotlib.pyplot as plt  `
* `import matplotlib.pyplot as plt`

## python general

Python matplotlib example: general

```python
import matplotlib.pyplot as plt
plt.scatter(x,y)
plt.xlabel(u"时间")
plt.ylabel(u"销量")
plt.xticks(np.arange(min(x), max(x)+1, 1.0))
plt.xlim(0,1000) 
plt.xticks(fontsize=10)
plt.rcParams['font.sans-serif'] = ['SimHei']
plt.rcParams['axes.unicode_minus']=False
plt.rcParams['figure.figsize'] = 15, 6
plt.legend(loc='upper right')
plt.title()
plt.grid(True)
plt.vlines(600, 0,80, colors = "r", linestyles = "dashed")
plt.hlines(20, 400,800, colors = "c", linestyles = "dashed")
plt.annotate(alarm[result[i]], xy=(i,timeSeries[i]), xytext=(i, timeSeries[i]+1),
             arrowprops=dict(facecolor=marker[result[i]], shrink=0.05, arrowstyle='->'))
plt.show()

# 画子图
import matplotlib.pyplot as plt  
x = np.arange(0, 100)  
plt.subplot(221)  
plt.plot(x, x)  
plt.subplot(222)  
plt.plot(x, -x)  
plt.subplot(223)  
plt.plot(x, x ** 2)  
plt.subplot(224)  
plt.plot(x, np.log(x))  
plt.show()

# 双y轴
ax1 = fig.add_subplot(4, 2, i)
ax1.plot(x, y, 'r:')
ax1.legend(["failure rate"], loc="upper right")
ax2 = ax1.twinx()
ax2.plot(x, z, 'b--')
ax2.legend(["survival rate"], loc="upper right")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
