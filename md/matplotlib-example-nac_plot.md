---
title: matplotlib example nac plot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'nac plot'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python nac plot

Python matplotlib example: nac plot

```python
'''
NAC 3-D Plot Code
'''


# import statements
from mpl_toolkits.mplot3d import Axes3D
import matplotlib.pyplot as plt
from matplotlib import cm
from matplotlib.ticker import LinearLocator, FormatStrFormatter
import numpy as np


# 3-D axis creation
fig = plt.figure()
ax = fig.gca(projection='3d')


# Two functions for multiplication, and creating transformation matrix W
sigmoid = lambda y: 1 / (1 + np.exp(-y))
tanh = lambda x: ((np.exp(x)-np.exp(-x))/(np.exp(x)+np.exp(-x)))


# Make data
X = np.arange(-10, 10, 0.25)
Y = np.arange(-10, 10, 0.25)
X, Y = np.meshgrid(X, Y)
# Final function for Transformation Matrix W
Z = tanh(X)*sigmoid(Y)


# Axis Labelling
ax.set_xlabel('tanh(x)', fontsize=14)
ax.set_ylabel('sigmoid(y)', fontsize=14)
ax.set_zlabel('W', fontsize=14)


# Plot the surface.
surf = ax.plot_surface(X, Y, Z, cmap=cm.coolwarm,
                       linewidth=0, antialiased=False)

# Customize the z axis.
ax.set_zlim(-1.11, 1.11)
ax.zaxis.set_major_locator(LinearLocator(10))
ax.zaxis.set_major_formatter(FormatStrFormatter('%.02f'))

# Add a color bar which maps values to colors.
fig.colorbar(surf, shrink=0.5, aspect=5)
# show plot
plt.show()
# save plot in given directory
fig.savefig('NAC.png')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
