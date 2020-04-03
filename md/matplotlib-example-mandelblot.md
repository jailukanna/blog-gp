---
title: matplotlib example mandelblot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mandelblot'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python mandelblot

Python matplotlib example: mandelblot

```python
import numpy as np
import matplotlib.pyplot as plt

xmin, xmax, ymin, ymax = -2, 2, -2, 2
size = 1000 #点の数 size*size
n = 100 #発散判定の試行回数
X,Y = np.meshgrid(np.linspace(xmin,xmax,size),np.linspace(ymin,ymax,size))
C = X + 1j * Y;
Z = np.zeros([size, size], dtype=np.complex)
Res = np.zeros([size, size], dtype=np.int32)

for i in range(n):
    Z = Z**2 + C
    mask = np.absolute(Z) > 2
    Z[mask] = C[mask] = 0
    Res[mask] = i + 1

plt.pcolor(X, Y, Res)
plt.figure(figsize=(5, 5))
plt.show();

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
