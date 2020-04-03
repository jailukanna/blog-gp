---
title: matplotlib example nalu plot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'nalu plot'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python nalu plot

Python matplotlib example: nalu plot

```python
'''
NALU 3-D Plot Code
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


# Sub-function decleration for NALU units
sigmoid = lambda y: 1 / (1 + np.exp(-y))
tanh = lambda x: ((np.exp(x)-np.exp(-x))/(np.exp(x)+np.exp(-x)))
# learned gate
g_x = lambda x: 1 / (1 + np.exp(-x))
# small epsilon value
eps = 10e-2
# log-exponential space
m_y = lambda y: np.exp(np.log(np.abs(y)+eps))


# Make data
X = np.arange(-10, 10, 0.25)
Y = np.arange(-10, 10, 0.25)
X, Y = np.meshgrid(X, Y)

# Final function for Transformation Matrix W
W_nac = tanh(X)*sigmoid(Y)
# Final function for NALU
Z = ( (g_x(Y)*W_nac) + ((1-g_x(Y))*m_y(Y)) )


# Axis Labelling
ax.set_xlabel('tanh(x)', fontsize=12)
ax.set_ylabel('sig-M/G(y), exp(log(|y|+eps))', fontsize=12)
ax.set_zlabel('Y', fontsize=14)


# Plot the surface.
surf = ax.plot_surface(X, Y, Z, cmap=cm.coolwarm,
                       linewidth=0, antialiased=False)

# Customize the z axis.
ax.set_zlim(-1.1, 10.1)
ax.zaxis.set_major_locator(LinearLocator(10))
ax.zaxis.set_major_formatter(FormatStrFormatter('%.02f'))

# Add a color bar which maps values to colors.
fig.colorbar(surf, shrink=0.5, aspect=5)
# show plot
plt.show()
# save plot in given directory
fig.savefig('NALU.png')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
