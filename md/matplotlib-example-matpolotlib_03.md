---
title: matplotlib example matpolotlib 03 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matpolotlib 03'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python matpolotlib 03

Python matplotlib example: matpolotlib 03

```python
# 简单的图
import numpy as np
import matplotlib.pyplot as plt
# 从-1-----1之间等间隔采66个数.也就是说所画出来的图形是66个点连接得来的
x = np.linspace(-1, 1, 66)  # 起始值为-1，终止值为1，期间的点的个数为66
y1 = 2 * x + 1
plt.figure()
plt.plot(x, y1)
plt.show()


# 绘制x^2的图像
y2 = x ** 2
plt.plot(x, y2)
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
