---
title: matplotlib example matpolotlib 05 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matpolotlib 05'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python matpolotlib 05

Python matplotlib example: matpolotlib 05

```python
'''
    设置legend图例
'''

import matplotlib.pyplot as plt
import numpy as np

# 绘制简单图像
x = np.linspace(-1, 1, 50)
y1 = 2 * x + 1
y2 = x ** 2

plt.figure()
# 在绘制label是逗号（,）是必须的
l1, = plt.plot(x, y1, label='line')
l2, = plt.plot(x, y2, label='parabola', color='red', lw='1.0', linestyle='--')

# 设置坐标轴的取值范围
plt.xlim(-1, 1)
plt.ylim(0, 2)

# 设置坐标轴的label
plt.xlabel('X axis')
plt.ylabel('Y axis')

# 设置x轴的刻度
plt.xticks(np.linspace(-1, 1, 5))

#设置y轴的刻度和标签
plt.yticks([0, 0.5],['$minimum$', 'normal'])

# 设置legend
plt.legend(handles=[l1, l2], labels=['a', 'b'], loc='best')
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
