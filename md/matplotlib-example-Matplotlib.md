---
title: matplotlib example Matplotlib (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Matplotlib'

Functions in program: 
* `def f(t):`

Modules used in program: 
* `import os`
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python Matplotlib

Python matplotlib example: Matplotlib

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

'''
Visto en: https://matplotlib.org/gallery/lines_bars_and_markers/arctest.html#sphx-glr-gallery-lines-bars-and-markers-arctest-py
'''

# Matplotlib
import matplotlib.pyplot as plt
import numpy as np
import os

def f(t):
    'A damped exponential'
    s1 = np.cos(2 * np.pi * t)
    e1 = np.exp(-t)
    return s1 * e1


os.system('cls')
t1 = np.arange(0.0, 5.0, .2)
l = plt.plot(t1, f(t1), 'ro')
plt.setp(l, markersize=30)
plt.setp(l, markerfacecolor='C0')
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
