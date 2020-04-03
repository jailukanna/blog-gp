---
title: matplotlib example ProjectionFunc (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'ProjectionFunc'

Functions in program: 
* `def Pole2Car(r, theta, phi):`
* `def ExFunc(v):`

Modules used in program: 
* `import numpy as np`
* `import matplotlib.animation as anim`
* `import mpl_toolkits.mplot3d as Axes3D`
* `import matplotlib.pyplot as plt`

## python ProjectionFunc

Python matplotlib example: ProjectionFunc

```python

import matplotlib.pyplot as plt
import mpl_toolkits.mplot3d as Axes3D
import matplotlib.animation as anim
from matplotlib.animation import PillowWriter
import numpy as np
from numpy import sin, cos, pi


def ExFunc(v):
    A = np.array([[1, 0, 0], [0, 1, 0], [0, 0, 0]])
    v_out = A @ v
    return v_out


def Pole2Car(r, theta, phi):
    x = r*cos(theta)*cos(phi)
    y = r*sin(theta)*cos(phi)
    z = r*sin(phi)
    return np.array([[x], [y], [z]])


r = 1
phi = pi/6
T = np.linspace(0, 2, 120)
X_zero = (0, 0)
Y_zero = (0, 0)
Z_zero = (0, 0)

fig = plt.figure(figsize=(6, 6))
ax = fig.add_subplot(111, projection='3d')
ax.set_title("Projection Func")
ax.set_xlabel("x", size=14)
ax.set_ylabel("y", size=14)
ax.set_zlabel("z", size=14)
ax.set_xlim(-1.1, 1.1)
ax.set_ylim(-1.1, 1.1)
ax.set_zlim(-0.1, 1.1)

imgs = []

for t in T:
    theta = 2*pi*(t/2)
    V_in = Pole2Car(r, theta, phi)
    v_in = V_in.flatten()
    V_out = ExFunc(V_in)
    v_out = V_out.flatten()
    X = (v_in[0], v_out[0])
    Y = (v_in[1], v_out[1])
    Z = (v_in[2], v_out[2])
    im = ax.quiver(X_zero, Y_zero, Z_zero, X, Y,  Z,
                   length=r, arrow_length_ratio=0.1,
                   color=('blue', 'red'))
    imgs.append([im])

ani = anim.ArtistAnimation(fig, imgs, interval=66)
plt.show()

ani.save("PeojectionFuncEx.gif", writer="pillow")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
