---
title: matplotlib example surface (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'surface'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python surface

Python matplotlib example: surface

```python
import matplotlib.pyplot as plt
from matplotlib import cm
from matplotlib.ticker import LinearLocator, FormatStrFormatter
from mpl_toolkits.mplot3d import Axes3D
import numpy as np

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
X = np.arange(0, 1, .1)
Y = np.arange(0, 1, .1)
X, Y = np.meshgrid(X, Y)
Z = X**2 + Y**2

surf = ax.plot_surface(X, Y, Z, rstride=1, cstride=1, cmap=cm.coolwarm,
                        linewidth=0, antialiased=False)

ax.zaxis.set_major_locator(LinearLocator(10))
ax.zaxis.set_major_formatter(FormatStrFormatter('%.02f'))

fig.colorbar(surf, shrink=0.5, aspect=5)
plt.title('U(X, Y)')
plt.xlabel('X')
plt.ylabel('Y')
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
