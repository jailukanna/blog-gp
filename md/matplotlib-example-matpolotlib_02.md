---
title: matplotlib example matpolotlib 02 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matpolotlib 02'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python matpolotlib 02

Python matplotlib example: matpolotlib 02

```python
import numpy as np
import matplotlib.pyplot as plt

# 折线图
x = np.array([1, 2, 3, 4, 5, 6, 7, 8])
y = np.array([2, 4, 6, 8, 10, 12, 14, 16])
plt.plot(x, y, 'b', lw=5)  # x代表的是x轴，y代表的是Y轴，b代表的是颜色 lw是线宽 plot是折线图  (x,y)构成坐标，x与y一一对应


# 柱状图
x = np.array([1,2,3,4,5,6,7,8])
y = np.array([13,25,17,36,21,16,10,15])
plt.bar(x, y)  # x与y一一对应
plt.show()  # 将图片进行显示

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
