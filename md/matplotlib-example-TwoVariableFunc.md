---
title: matplotlib example TwoVariableFunc (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'TwoVariableFunc'

Functions in program: 
* `def FuncPlot2(f, x, y):`
* `def ExFunc(x, y):`

Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python TwoVariableFunc

Python matplotlib example: TwoVariableFunc

```python
import matplotlib.pyplot as plt
import numpy as np
from mpl_toolkits.mplot3d import Axes3D


def ExFunc(x, y):
    return -x ** 2-y ** 2+1


def FuncPlot2(f, x, y):
    out = np.zeros((len(x), len(y)))
    for i_y, p_y in enumerate(y):
        for i_x, p_x in enumerate(x):
            out[i_x, i_y] = f(p_x, p_y)
    return out


Interval = np.array([-5, 5])
Nsamp = 100
x = np.linspace(-5, 5, Nsamp)
y = np.linspace(-5, 5, Nsamp)
h = FuncPlot2(ExFunc, x, y)

X, Y = np.meshgrid(x, y)

figure = plt.figure()
ax = figure.add_subplot(111, projection='3d')
ax.set_xlabel("x", size=14)
ax.set_ylabel("y", size=14)
ax.set_zlabel("h", size=14)
surf = ax.plot_surface(X, Y, h, cmap="bwr", linewidth=0)
figure.colorbar(surf)
ax.set_title("TwoVariableFunc")
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
