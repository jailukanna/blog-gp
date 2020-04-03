---
title: matplotlib example contour-example (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'contour-example'

Functions in program: 
* `def f(x, y):`

## python contour-example

Python matplotlib example: contour-example

```python
def f(x, y):
    return np.sin(x) ** 10 + np.cos(10+x*y) + np.cos(x)
  
x = np.linspace(0, 5, 50)
y = np.linspace(0, 5, 50)
X, Y = np.meshgrid(x, y)
Z = f(X, Y)

# draw contour
contour = plt.contour(X, Y, Z, 3, colors='black')

# add z value into contour
plt.clabel(contour, fontsize=10, inline=True)

# plus transparent bg from imshow
# RdGy => RedGray
plt.imshow(Z, extent=[0, 5.0, 0, 5.0], origin='lower', cmap=plt.cm.RdGy, alpha=0.3)

# show color bar
plt.colorbar()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
