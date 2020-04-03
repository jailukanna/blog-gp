---
title: matplotlib example matpolotlib 04 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matpolotlib 04'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python matpolotlib 04

Python matplotlib example: matpolotlib 04

```python
'''
    绘制坐标轴
'''

import numpy as np
import matplotlib.pyplot as plt

# 绘制简单图形
x = np.linspace(-1, 1, 50)
y1 = 2 * x + 1
y2 = x ** 2
plt.figure()
plt.plot(x, y1)
plt.plot(x, y2, 'r', lw=1.0, linestyle='--')

# 设置坐标轴的取值范围
plt.xlim(-1, 1)
plt.ylim(0, 3)


# 设置坐标轴的lable
plt.xlabel(u'这是X轴', fontproperties='SimHei', fontsize=14)
plt.ylabel(u'这是Y轴', fontproperties='SimHei', fontsize=14)

#更改x轴坐标刻度
plt.xticks(np.linspace(-1, 1, 5))


# 获取当前的坐标轴
ax = plt.gca()
# print(ax)
# print(type(ax))
# 设置右边框和上边框
ax.spines['right'].set_color('none')
ax.spines['top'].set_color('none')

# 设置x轴为下边框
ax.xaxis.set_ticks_position('bottom')
# 设置y轴为左边框
ax.yaxis.set_ticks_position('left')

# 设置x轴，y轴在(0,0)位置
#ax.spines['bottom'].set_position(('data', 0))
ax .spines['left'].set_position(('data', 0))

# 设置坐标轴label的大小，背景色等信息
for label in ax.get_xticklabels() + ax.get_yticklabels():
    label.set_fontsize(12)
    label.set_bbox(dict(facecolor='green', alpha=0.7))

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
