---
title: matplotlib example demo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'demo'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python demo

Python matplotlib example: demo

```python
import numpy as np
import matplotlib.pyplot as plt
from betterstep import betterstep

x = np.random.uniform(0, 2*np.pi, 500)
y = np.sin(x) + np.random.randn(len(x))
bins = np.linspace(0, 2*np.pi, 25)
num, _ = np.histogram(x, bins, weights=y)
denom, _ = np.histogram(x, bins)

plt.plot(x, y, ".k", alpha=0.2)
betterstep(bins, num / denom, linewidth=2)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
