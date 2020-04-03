---
title: matplotlib example mpl event 1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mpl event 1'

Functions in program: 
* `def onclick(event):`

Modules used in program: 
* `import scipy`
* `import matplotlib`
* `import ipywidgets`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python mpl event 1

Python matplotlib example: mpl event 1

```python
#jupyterlab
%matplotlib widget 
#jupyter notebook
#%matplotlib notebook 

import numpy as np
import matplotlib.pyplot as plt
from ipywidgets import Output
from scipy import ndimage
from mpl_toolkits.axes_grid1 import make_axes_locatable
plt.rcParams['font.size']=12
plt.rcParams['font.family'] = 'sans-serif'

#make data
n = 1
s = 10
im = np.zeros((s, s))
points = (s*np.random.random((2, n**2))).astype(int)
im[points[0], points[1]] = 1000
im = ndimage.gaussian_filter(im, sigma=s/(5.*n))

#show
fig, ax = plt.subplots(figsize=(4,4))
img = ax.imshow(im, cmap=plt.cm.gist_earth, interpolation='nearest',origin='upper',alpha=1)

#colorbar
divider = make_axes_locatable(ax)
cax = divider.append_axes("top", size="5%", pad=0.1)
plt.colorbar(img, cax=cax,orientation='horizontal')
cax.xaxis.set_ticks_position('top')

ax.set_xlabel("x")
ax.set_ylabel("y")
plt.tight_layout()

out = Output()
display(out)

@out.capture(clear_output=True)

def onclick(event):
    print('%s click: button=%d, x=%d, y=%d,z=%f, xdata=%f, ydata=%f' %
          ('double' if event.dblclick else 'single', event.button,
           event.x, event.y,
           im[int(np.rint(event.ydata)),int(np.rint(event.xdata))],
           event.xdata, event.ydata))
        
cid = fig.canvas.mpl_connect('button_press_event', onclick)



fig.canvas.mpl_disconnect(cid)
 
    
#version    
import ipywidgets
print(ipywidgets.__version__)
#7.5.1
import matplotlib
print(matplotlib.__version__)
#3.2.0
import scipy
print(scipy.__version__)
#1.4.1
print(np.__version__)
#1.18.1

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
