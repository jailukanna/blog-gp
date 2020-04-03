---
title: matplotlib example matplotlib 3d Scatterdemo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib 3d Scatterdemo'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python matplotlib 3d Scatterdemo

Python matplotlib example: matplotlib 3d Scatterdemo

```python
"""
==========================
Scatter plot on polar axis
==========================

Size increases radially in this example and color increases with angle
(just to verify the symbols are being scattered correctly).
"""
import numpy as np
import matplotlib.pyplot as plt

data = np.loadtxt(open("data.csv","rb"),delimiter=",",skiprows=0)
data=data.transpose()
print(type(data.shape))
# Fixing random state for reproducibility
np.random.seed(19680801)

# Compute areas and colors
N = 150
r = np.linspace(0,5995,1200)
print('-------------r：-----------------------------')
print(r)
print('--------------theta：----------------------------')
theta = np.linspace(0,300,1200)
print(theta)
print('----------------area：----------------------------')

area = r = np.linspace(1,5,1200)

print(area)



fig = plt.figure()
ax = fig.add_subplot(111, projection='polar')
c = ax.scatter(theta, r, s=area, cmap='RdBu', alpha=0.75)

###############################################################################
# Scatter plot on polar axis, with offset origin
# ----------------------------------------------
#
# The main difference with the previous plot is the configuration of the origin
# radius, producing an annulus. Additionally, the theta zero location is set to
# rotate the plot.

fig = plt.figure()
ax = fig.add_subplot(111, polar=True)
c = ax.scatter(theta, r,  s=area, cmap='hsv', alpha=0.75)

ax.set_rorigin(-2.5)
ax.set_theta_zero_location('W', offset=10)

###############################################################################
# Scatter plot on polar axis confined to a sector
# -----------------------------------------------
#
# The main difference with the previous plots is the configuration of the
# theta start and end limits, producing a sector instead of a full circle.

fig = plt.figure()
ax = fig.add_subplot(111, polar=True)
c = ax.scatter(theta, r, s=area, cmap='hsv', alpha=0.75)

fig = plt.figure()
ax = fig.add_subplot(111, polar=True)
c = ax.scatter(theta, r,  s=area, cmap='hsv', alpha=0.75)



plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
