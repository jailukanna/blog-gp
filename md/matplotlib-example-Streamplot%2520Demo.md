---
title: matplotlib example Streamplot%2520Demo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Streamplot%2520Demo'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python Streamplot%2520Demo

Python matplotlib example: Streamplot%2520Demo

```python
# coding: utf-8
'''
Demo of the `streamplot` function.

A streamplot, or streamline plot, is used to display 2D vector fields.
'''

import numpy as np
import matplotlib.pyplot as plt

Y, X = np.mgrid[-3:3:100j, -3:3:100j]
U = -1 - X**2 + Y
V = 1 + X - Y**2
speed = np.sqrt(U*U + V*V)
plt.streamplot(X, Y, U, V, color=U, linewidth=2, cmap=plt.cm.autumn)
plt.colorbar()
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
