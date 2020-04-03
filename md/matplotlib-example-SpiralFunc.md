---
title: matplotlib example SpiralFunc (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'SpiralFunc'

Functions in program: 
* `def p_z(t):`
* `def p_y(t):`
* `def p_x(t):`
* `def ExFunc(t):`

Modules used in program: 
* `import numpy as np`
* `import matplotlib.animation as anim`
* `import mpl_toolkits.mplot3d as Axes3D`
* `import matplotlib.pyplot as plt`

## python SpiralFunc

Python matplotlib example: SpiralFunc

```python

import matplotlib.pyplot as plt
import mpl_toolkits.mplot3d as Axes3D
import matplotlib.animation as anim
from matplotlib.animation import PillowWriter
import numpy as np
from numpy import sin, cos, pi


def ExFunc(t):
    return p_x(t), p_y(t), p_z(t)


def p_x(t):
    return cos(2*pi*0.2*t)


def p_y(t):
    return sin(2*pi*0.2*t)


def p_z(t):
    return t


T_end = np.linspace(0, 10, 100)

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
ax.set_title("Spiral Func")
ax.set_xlabel("x", size=14)
ax.set_ylabel("y", size=14)
ax.set_zlabel("z", size=14)

imgs = []

for t_end in T_end:
    t = np.linspace(0, t_end, t_end*10)
    im = ax.plot(*ExFunc(t), color="blue")
    imgs.append(im)

ani = anim.ArtistAnimation(fig, imgs, interval=100)
# plt.show()

# 保存用に使用
#ani.save("VectorFuncEx.gif", writer="pillow")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
