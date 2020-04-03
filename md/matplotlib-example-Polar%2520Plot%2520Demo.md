---
title: matplotlib example Polar%2520Plot%2520Demo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Polar%2520Plot%2520Demo'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python Polar%2520Plot%2520Demo

Python matplotlib example: Polar%2520Plot%2520Demo

```python
# coding: utf-8
# Demo of a line plot on a polar axis.

import numpy as np
import matplotlib.pyplot as plt

r = np.arange(0, 3.0, 0.01)
theta = 2 * np.pi * r

ax = plt.subplot(111, polar=True)
ax.plot(theta, r, color='r', linewidth=3)
ax.set_rmax(2.0)
ax.grid(True)

ax.set_title("A line plot on a polar axis", va='bottom')
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
