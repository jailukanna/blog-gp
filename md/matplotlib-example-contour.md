---
title: matplotlib example contour (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'contour'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python contour

Python matplotlib example: contour

```python
import matplotlib.pyplot as plt
from matplotlib.ticker import MaxNLocator
import numpy as np

X = np.arange(0, 1, .1)
Y = np.arange(0, 1, .1)
X, Y = np.meshgrid(X, Y)
Z = X**2 + Y**2

levels = MaxNLocator(nbins=15).tick_values(Z.min(), Z.max())
cmap = plt.get_cmap('PiYG')

plt.contourf(X, Y, Z, levels=levels, cmap=cmap)
plt.colorbar()
plt.title('U(X, Y)')
plt.xlabel('X')
plt.ylabel('Y')
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
