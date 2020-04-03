---
title: matplotlib example Matplotlib r1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Matplotlib r1'

Functions in program: 
* `def fun(n):`

Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib as mpl`

## python Matplotlib r1

Python matplotlib example: Matplotlib r1

```python
import matplotlib as mpl
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
import numpy as np
mpl.use('macosx')
plt.style.use('dark_background')

n = range(1,7)
x = np.linspace(-1, 1, 100)
def fun(n):
    x = np.linspace(-1, 1, 100)
    y = x**n
    return y


# plt.ylabel('some numbers')
n_col=len(list(n))
fig, ax = plt.subplots(1, 1, sharex=True, sharey=True, figsize=(8, 6))
# for i in n:
#     ax.plot(x,fun(n))
for i in n:
    for j in ax:
        # print(j)
        j.plot(x,fun(i))
        j.grid(ls='--', lw=0.5, c='gray')
        j.set_ylim(-1, 1)
        j.set_xlim(-1, 1)
        j.spines['left'].set_position('center')
        j.spines['right'].set_color('none')
        j.spines['bottom'].set_position('center')
        j.spines['top'].set_color('none')
        j.arrow()
        # j.spines['left'].set_smart_bounds(True)
        # j.spines['bottom'].set_smart_bounds(True)
        # j.xaxis.set_ticks_position('bottom')
        # j.yaxis.set_ticks_position('left')



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
