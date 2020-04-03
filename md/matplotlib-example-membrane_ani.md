---
title: matplotlib example membrane ani (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'membrane ani'

Functions in program: 
* `def data_gen(num):`

Modules used in program: 
* `import matplotlib.animation as animation`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python membrane ani

Python matplotlib example: membrane ani

```python
import numpy as np
from mpl_toolkits.mplot3d import Axes3D
import matplotlib.pyplot as plt
import matplotlib.animation as animation

# In Windows the next line should provide the full path to convert.exe
# since convert is a Windows command
#rcParams['animation.convert_path'] = "C:\Program Files\ImageMagick-6.9.3\convert.exe"


fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
plot_args = {'rstride': 1, 'cstride': 1, 'cmap':"Spectral",
             'linewidth': 0.1, 'antialiased': True, 'edgecolor': '#1e1e1e',
             'shade': True, 'alpha': 1.0, 'vmin': -1, 'vmax':1}
x, y = np.mgrid[0:1:31j, 0:1:31j]


def data_gen(num):
    ax.cla()
    z = np.sin(3*np.pi*x)*np.sin(2*np.pi*y)*np.sin(0.1*np.pi*num)
    surf = ax.plot_surface(x, y, z, **plot_args)
    ax.view_init(elev=35, azim=45)
    ax.set_xlim(0, 1)
    ax.set_ylim(0, 1)
    ax.set_zlim(-1, 1)
    ax.set_xlabel(r"$x$")
    ax.set_ylabel(r"$y$")
    ax.set_zlabel(r"$z$")
    ax.set_zticks([-1, -0.5, 0, 0.5, 1])
    return surf


ani = animation.FuncAnimation(fig, data_gen, range(30), blit=False)
ani.save("membrane-3-2.mp4")
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
