---
title: matplotlib example anim 3d matplotlib (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'anim 3d matplotlib'


Modules used in program: 
* `import matplotlib.colors as colors`
* `import matplotlib.animation as animation`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import numpy as np`

## python anim 3d matplotlib

Python matplotlib example: anim 3d matplotlib

```python
import numpy as np
from mpl_toolkits.mplot3d import axes3d

import matplotlib
matplotlib.use("Agg")

import matplotlib.pyplot as plt
import matplotlib.animation as animation
import matplotlib.colors as colors


# # Set up formatting for the movie files
Writer = animation.writers['ffmpeg']
writer = Writer(fps=15, metadata=dict(artist='Me'), bitrate=1800)

fig = plt.figure()
fig.add_subplot(111, projection='3d')
# ax = plt.gca()
# ps = plt.gca().plot_surface(X, Y, Z,cmap='viridis', shade=False,  rcount=200, ccount=500,
#                             edgecolor='k', linewidth=0.01, zorder=100, rasterized=True)

x = np.arange(-9, 10)
y = np.arange(-9, 10).reshape(-1, 1)
base = np.hypot(x, y)
ims = []
for add in np.arange(15):
    ims.append((plt.gca().plot_surface(x, y, base + add, norm=plt.Normalize(0, 30)),))
    
    
im_ani = animation.ArtistAnimation(fig, ims, interval=50, repeat_delay=3000,
                                   blit=True, repeat=True)
im_ani.save('im.mp4', writer=writer)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
