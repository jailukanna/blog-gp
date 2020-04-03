---
title: matplotlib example batman curve (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'batman curve'

Functions in program: 
* `def g(a):`

Modules used in program: 
* `import matplotlib.font_manager as font_manager`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python batman curve

Python matplotlib example: batman curve

```python
"""
Be sure that Humor Sans is installed. In Ubuntu, you can do:

1. Install Comic sans: sudo apt install fonts-humor-sans
2. Remove matplotlib cache: rm ~/.cache/matplotlib -r

Taken from:
    https://stackoverflow.com/a/49408754/3358223

"""
from __future__ import division
import numpy as np
from numpy import abs, sqrt
import matplotlib.pyplot as plt
import matplotlib.font_manager as font_manager

path = './Humor-Sans.ttf'
prop = font_manager.FontProperties(fname=path)

def g(a):
    return sqrt(abs(a)/a)
x, y = np.mgrid[-7:7:501j, -3:3:501j]


eq1 = (x/7)**2 * g(abs(x) - 3) + (y/3)**2*g(y + 3/7*sqrt(33)) - 1
eq2 = abs(x/2) - x**2*(3*sqrt(33) - 7)/112 - 3 + \
      sqrt(1 - (abs(abs(x)- 2) - 1)**2) - y
eq3 = 9*g((1 - abs(x))*(abs(x) - 3/4)) - 8*abs(x) - y
eq4 = 3*abs(x) + 3/4*g((3/4 - abs(x))*(abs(x) - 1/2)) - y
eq5 = 6/7*sqrt(10) + (3/2 - abs(x)/2) * g((abs(x) - 1)) - 6/14*sqrt(10) * \
      sqrt(4 - (abs(x) - 1)**2) - y
eq6 = 9/4*sqrt(abs((x - 1/2)*(x + 1/2))/((1/2 - x)*(1/2 + x))) - y

eqs = [eq1, eq2, eq3, eq4, eq5, eq6]
with plt.xkcd():
    for eq in eqs:
        C = plt.contour(x, y, eq, [0], linewidths=0)
        coords = C.allsegs[0]
        for coord in coords:
            plt.plot(coord[:, 0], coord[:, 1], 'k', lw=2)
    plt.axis("scaled")
    plt.xlim([-8, 8])
    plt.ylim([-4, 4])
    plt.savefig("Batman curve.png", dpi=600, bbox_inches="tight")
    plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
