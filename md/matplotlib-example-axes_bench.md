---
title: matplotlib example axes bench (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'axes bench'

Functions in program: 
* `def plot(axes):`

Modules used in program: 
* `import platform`
* `import numpy as np`
* `import time`
* `import thin_axes`
* `import matplotlib.pyplot as plt`
* `import matplotlib`

## python axes bench

Python matplotlib example: axes bench

```python
#!/usr/bin/env python
#coding=utf-8

import matplotlib
matplotlib.use("Agg")
import matplotlib.pyplot as plt
import thin_axes

import time
import numpy as np

import platform

print(" ".join(platform.uname()))

def plot(axes):
    for a in axes.flatten():
        x, y = np.random.randn(2, 5000)
        a.plot(x, y, '.')


start_time = time.time()
fig, axes = plt.subplots(20,20)
plt.savefig('test.png')
print("Standard axes {} s".format(time.time() - start_time))
plt.close()

start_time = time.time()
fig, axes = plt.subplots(20,20, subplot_kw=dict(projection='thin'))
plt.savefig('test.png')
print("Thin axes {} s".format(time.time() - start_time))
plt.close()

start_time = time.time()
fig, axes = plt.subplots(20,20)
plt.savefig('test.png')
print("Standard axes with plotting {} s".format(time.time() - start_time))
plt.close()

start_time = time.time()
fig, axes = plt.subplots(20,20, subplot_kw=dict(projection='thin'))
plot(axes)
plt.savefig('test.png')
print("Thin axes with plotting {} s".format(time.time() - start_time))
plt.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
