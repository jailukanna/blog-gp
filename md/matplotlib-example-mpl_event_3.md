---
title: matplotlib example mpl event 3 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mpl event 3'

Functions in program: 
* `def gradient_image(ax, extent, direction=0.3, cmap_range=(0, 1), **kwargs):`

Modules used in program: 
* `import matplotlib`
* `import ipywidgets`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python mpl event 3

Python matplotlib example: mpl event 3

```python
#jupyterlab
%matplotlib widget
#jupyter notebook
#%matplotlib notebook
#GUI
#%matplotlib
import numpy as np
import matplotlib.pyplot as plt
from ipywidgets import Output
from matplotlib.patches import Rectangle


def gradient_image(ax, extent, direction=0.3, cmap_range=(0, 1), **kwargs):
    """
    Draw a gradient image based on a colormap.
    Parameters
    ----------
    ax : Axes
        The axes to draw on.
    extent
        The extent of the image as (xmin, xmax, ymin, ymax).
        By default, this is in Axes coordinates but may be
        changed using the *transform* kwarg.
    direction : float
        The direction of the gradient. This is a number in
        range 0 (=vertical) to 1 (=horizontal).
    cmap_range : float, float
        The fraction (cmin, cmax) of the colormap that should be
        used for the gradient, where the complete colormap is (0, 1).
    **kwargs
        Other parameters are passed on to `.Axes.imshow()`.
        In particular useful is *cmap*.
    """
    phi = direction * np.pi / 2
    v = np.array([np.cos(phi), np.sin(phi)])
    X = np.array([[v @ [1, 0], v @ [1, 1]],
                  [v @ [0, 0], v @ [0, 1]]])
    a, b = cmap_range
    X = a + (b - a) / X.max() * X
    im = ax.imshow(X, extent=extent, interpolation='gaussian',
                   vmin=0, vmax=1, **kwargs)
    return im  

#show
fig, ax = plt.subplots()

xmin, xmax = xlim = 0, 2
ymin, ymax = ylim = 0, 1
ax.set(xlim=xlim, ylim=ylim, autoscale_on=False)
ax.set_aspect('equal')
ax.axis('off')

# background image
gradient_image(ax, direction=1, extent=(0, 1, 0, 1), transform=ax.transAxes,
               cmap=plt.cm.Greys, cmap_range=(0.9, 0.1))

#plot Rectangle
rect = Rectangle((0.9, 0.4),0.2,0.2,color='0.5')
ax.add_patch(rect)
#plt.savefig("draggable_rect.png",dpi=100)
plt.show()


#play
out = Output()
display(out)

class DraggableRectangle:
    def __init__(self, rect):
        self.rect = rect
        self.press = None

    def connect(self):
        'connect to all the events we need'
        self.cidpress = self.rect.figure.canvas.mpl_connect(
            'button_press_event', self.on_press)
        self.cidrelease = self.rect.figure.canvas.mpl_connect(
            'button_release_event', self.on_release)
        self.cidmotion = self.rect.figure.canvas.mpl_connect(
            'motion_notify_event', self.on_motion)
    
    @out.capture(clear_output=True)
    def on_press(self, event):
        'on button press we will see if the mouse is over us and store some data'
        if event.inaxes != self.rect.axes: return

        contains, attrd = self.rect.contains(event)
        if not contains: return
        print('event contains', self.rect.xy)
        x0, y0 = self.rect.xy
        self.press = x0, y0, event.xdata, event.ydata
    
           
    def on_motion(self, event):
        'on motion we will move the rect if the mouse is over us'
        if self.press is None: return
        if event.inaxes != self.rect.axes: return
        x0, y0, xpress, ypress = self.press
        dx = event.xdata - xpress
        dy = event.ydata - ypress
        #print('x0=%f, xpress=%f, event.xdata=%f, dx=%f, x0+dx=%f' %
        #      (x0, xpress, event.xdata, dx, x0+dx))
        self.rect.set_x(x0+dx)
        self.rect.set_y(y0+dy)

        self.rect.figure.canvas.draw()


    def on_release(self, event):
        'on release we reset the press data'
        self.press = None
        self.rect.figure.canvas.draw()

    def disconnect(self):
        'disconnect all the stored connection ids'
        self.rect.figure.canvas.mpl_disconnect(self.cidpress)
        self.rect.figure.canvas.mpl_disconnect(self.cidrelease)
        self.rect.figure.canvas.mpl_disconnect(self.cidmotion)

fig, ax = plt.subplots()

xmin, xmax = xlim = 0, 2
ymin, ymax = ylim = 0, 1
ax.set(xlim=xlim, ylim=ylim, autoscale_on=False)
ax.set_aspect('equal')
ax.axis('off')

# background image
gradient_image(ax, direction=1, extent=(0, 1, 0, 1), transform=ax.transAxes,
               cmap=plt.cm.Greys, cmap_range=(0.9, 0.1))

rects = Rectangle((0.9, 0.4),0.2,0.2,color='0.5')
ax.add_patch(rects)
dr = DraggableRectangle(rects)
dr.connect()
plt.show()

import ipywidgets
print(ipywidgets.__version__)
#7.5.1
import matplotlib
print(matplotlib.__version__)
#3.2.0
print(np.__version__)
#1.18.1
 

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
