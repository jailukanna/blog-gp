---
title: matplotlib example plots (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plots'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python plots

Python matplotlib example: plots

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib import rcParams

rcParams['font.family'] = 'serif'
rcParams['font.size'] = 16

x = np.pi*np.linspace(-1, 1, 200)
y = np.sin(x)
plt.figure(figsize=(8,6))
plt.plot(x/np.pi,np.pi*y)
plt.grid(True, color="blue", alpha=0.3)
plt.xlabel(r"$x/\pi$", size=18)
plt.ylabel(r"$\pi y$", size=18)

x = np.pi*np.linspace(-1, 1, 200)
y = np.pi*np.linspace(-1, 1, 200)
X,Y = np.meshgrid(x,y)
Z = np.sin(np.sqrt(X**2 + Y**2))/np.sqrt(X**2 + Y**2)
plt.figure(figsize=(8,8))
plt.contourf(X,Y,Z, cmap="RdBu")
plt.xlabel(r"$x$", size=18)
plt.ylabel(r"$y$", size=18)

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
