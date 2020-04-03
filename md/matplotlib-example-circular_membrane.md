---
title: matplotlib example circular membrane (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'circular membrane'

Functions in program: 
* `def data_gen(num, m, n):`

Modules used in program: 
* `import matplotlib.animation as animation`
* `import matplotlib.pyplot as plt`

## python circular membrane

Python matplotlib example: circular membrane

```python
from __future__ import division
from numpy import pi, sin, cos, mgrid
from scipy.special import jn, jn_zeros
from mpl_toolkits.mplot3d import Axes3D
import matplotlib.pyplot as plt
import matplotlib.animation as animation
from matplotlib import rcParams


# In Windows the next line should provide the full path to convert.exe
# since convert is a Windows command
rcParams['animation.convert_path'] = "C:\Program Files\ImageMagick-6.9.3\convert.exe"
rcParams['savefig.transparent'] = True
plot_args = {'rstride': 1, 'cstride': 1, 'cmap':"RdYlBu",
             'linewidth': 0.2, 'antialiased': True, 'color': '#1e1e1e',
             'shade': True, 'alpha': 1.0, 'vmin': -1, 'vmax':1}


def data_gen(num, m, n):
    ax.cla()
    lam = jn_zeros(m, n)[-1]
    dt = 2*pi/(30*lam)
    z = cos(m*t)*jn(m, lam*r)*sin(lam*num*dt)
    surf = ax.plot_surface(x, y, z, **plot_args)
    ax.view_init(elev=30, azim=45)
    ax.set_xlim(-0.6, 0.6)
    ax.set_ylim(-0.6, 0.6)
    ax.set_zlim(-1, 1)    
    plt.axis("off")
    return surf


r, t = mgrid[0:1:20j, 0:2*pi:40j]
x, y = r*cos(t), r*sin(t)
for m in range(0, 3):
    for n in range(1, 4):
        fig = plt.figure(dpi=600, facecolor=None)
        ax = fig.add_subplot(111, projection='3d')
        ani = animation.FuncAnimation(fig, data_gen, range(30), blit=False,
                              fargs=(m, n))
#        ani.save("Drum vibration mode01.gif", writer='imagemagick')
        ani.save("Drum vibration mode%i%i.avi" % (m, n), writer='ffmpeg')
#        plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
