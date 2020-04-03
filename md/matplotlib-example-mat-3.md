---
title: matplotlib example mat-3 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mat-3'

Functions in program: 
* `def update_line(num, x, y, z, l):`

Modules used in program: 
* `import matplotlib.animation as animation`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import matplotlib as mpl`

## python mat-3

Python matplotlib example: mat-3

```python
import matplotlib as mpl
from mpl_toolkits.mplot3d import Axes3D
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as animation

def update_line(num, x, y, z, l):
    x.append(1.0)
    y.append(2.0)
    z.append(3.0)
    print(x, y, z)
    l, = ax.plot(x, y, z, label='Line')
    return l,

fig = plt.figure()
ax = fig.gca(projection='3d')
ax.set_xlabel('X')
ax.set_ylabel('Y')
ax.set_zlabel('Z')
ax.legend()

x = [1.0, 2.0, 3.0]
y = [4.0, 7.0, 8.0]
z = [6.0, 9.0, 5.0]

l, = ax.plot(x, y, z, label='Line')

line_ani = animation.FuncAnimation(fig, update_line, 25, fargs=(x, y, z, l), interval=2000, blit=True)

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
