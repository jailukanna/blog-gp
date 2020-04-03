---
title: matplotlib example surface plot shade (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'surface plot shade'

Functions in program: 
* `def animate(i):`
* `def animate(i):`
* `def animate(i):`

Modules used in program: 
* `import matplotlib.animation as animation`
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python surface plot shade

Python matplotlib example: surface plot shade

```python
#surface plot shade
%matplotlib inline
"""
=======================================
Custom hillshading in a 3D surface plot
=======================================

Demonstrates using custom hillshading in a 3D surface plot.
"""

from mpl_toolkits.mplot3d import Axes3D
from matplotlib import cm
from matplotlib.colors import LightSource
import matplotlib.pyplot as plt
import numpy as np
file = 'jacksboro_fault_dem.npz'

# Load and format data
with    np.load(file) as dem:
    z = dem['elevation']
    nrows, ncols = z.shape
    x = np.linspace(dem['xmin'], dem['xmax'], ncols)
    y = np.linspace(dem['ymin'], dem['ymax'], nrows)
    x, y = np.meshgrid(x, y)

x.shape
#(344, 403)

region = np.s_[5:50, 5:50]
region
#(slice(5, 50, None), slice(5, 50, None))

x, y, z = x[region], y[region], z[region]

# Set up plot
fig, ax = plt.subplots(subplot_kw=dict(projection='3d'))

ls = LightSource(270, 45)
rgb = ls.shade(z, cmap=cm.gist_earth, vert_exag=0.1, blend_mode='soft')
surf = ax.plot_surface(x, y, z, rstride=1, cstride=1, facecolors=rgb,
                       linewidth=0, antialiased=False, shade=False)
ax.set_xticklabels([])
ax.set_yticklabels([])
ax.set_zticklabels([])
plt.savefig('3d_surface_shade.jpg',dpi=120)
plt.show()

#change azdeg

from IPython.display import HTML
import matplotlib.animation as animation

def animate(i):
    ax.cla()
    ls = LightSource(7.2*i, 45)
    rgb = ls.shade(z, cmap=cm.gist_earth, vert_exag=0.1, blend_mode='soft')
    ax.plot_surface(x, y, z, rstride=1, cstride=1, facecolors=rgb,
                       linewidth=0, antialiased=False, shade=False)
    ax.set_title('azdeg='+str(7.2*i)[:5]+'')
    ax.set_xticklabels([])
    ax.set_yticklabels([])
    ax.set_zticklabels([])
    return fig,

ani = animation.FuncAnimation(fig, animate,frames=50, interval=100, blit=True)    
ani.save('shade_change_azdeg.mp4', writer="ffmpeg",dpi=120)
HTML(ani.to_html5_video())


#change altdeg

fig, ax = plt.subplots(subplot_kw=dict(projection='3d'))

def animate(i):
    ax.cla()
    ls = LightSource(315, 1.8*i)
    rgb = ls.shade(z, cmap=cm.gist_earth, vert_exag=0.1, blend_mode='soft')
    ax.plot_surface(x, y, z, rstride=1, cstride=1, facecolors=rgb,
                       linewidth=0, antialiased=False, shade=False)
    ax.set_title('altdeg='+str(1.8*i)[:4]+'')
    ax.set_xticklabels([])
    ax.set_yticklabels([])
    ax.set_zticklabels([])
    return fig,

ani = animation.FuncAnimation(fig, animate,frames=50, interval=100, blit=True)    
ani.save('shade_change_altdeg.mp4', writer="ffmpeg",dpi=120)
HTML(ani.to_html5_video())

#change vert_exag

fig, ax = plt.subplots(subplot_kw=dict(projection='3d'))

def animate(i):
    ax.cla()
    ls = LightSource(315, 45)
    rgb = ls.shade(z, cmap=cm.gist_earth, vert_exag=0.05*i, blend_mode='soft')
    ax.plot_surface(x, y, z, rstride=1, cstride=1, facecolors=rgb,
                       linewidth=0, antialiased=False, shade=False)
    ax.set_title('vert_exag='+str(0.05*i)[:5]+'')
    ax.set_xticklabels([])
    ax.set_yticklabels([])
    ax.set_zticklabels([])
    return fig,

ani = animation.FuncAnimation(fig, animate,frames=20, interval=250, blit=True)    
ani.save('shade_change_vert_exag.mp4', writer="ffmpeg",dpi=120)
HTML(ani.to_html5_video())

 

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
