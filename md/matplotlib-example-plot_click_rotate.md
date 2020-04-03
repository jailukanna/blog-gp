---
title: matplotlib example plot click rotate (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot click rotate'

Functions in program: 
* `def on_pick(event):`
* `def angle (p1, p2, p0):`
* `def rotate_vector(v, angle, anchor):`

Modules used in program: 
* `import matplotlib.patches as mpatches`
* `import matplotlib.path as mpath`
* `import math`
* `import csv`
* `import os`
* `import tkFileDialog`
* `import Tkinter as Tk`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import matplotlib.cbook as cbook`
* `import matplotlib.mlab as mlab`
* `import matplotlib.pyplot as plt`

## python plot click rotate

Python matplotlib example: plot click rotate

```python
import matplotlib.pyplot as plt
import matplotlib.mlab as mlab
import matplotlib.cbook as cbook
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.widgets import Button
import Tkinter as Tk
import tkFileDialog
import os
import csv
import math
import matplotlib.path as mpath
import matplotlib.patches as mpatches

def rotate_vector(v, angle, anchor):
    """Rotate a vector `v` by the given angle, relative to the anchor point."""
    x, y = v

    x = float(x) - anchor[0]
    y = float(y) - anchor[1]
    # Here is a compiler optimization; inplace operators are slower than
    # non-inplace operators like above. This function gets used a lot, so
    # performance is critical.
    
    cos_theta = math.cos(math.radians(angle))
    sin_theta = math.sin(math.radians(angle))

    nx = x*cos_theta - y*sin_theta
    ny = x*sin_theta + y*cos_theta

    nx = nx + anchor[0]
    ny = ny + anchor[1]
    return [nx, ny]

def angle (p1, p2, p0):
    #http://stackoverflow.com/questions/2827393/angles-between-two-n-dimensional-vectors-in-python
    
    v0 = np.array(p0) - np.array(p1)
    v1 = np.array(p2) - np.array(p1)
    angle = np.math.atan2(np.linalg.det([v0,v1]),np.dot(v0,v1))
    return np.degrees(angle)

def on_pick(event):
    artist = event.artist
    xmouse, ymouse = event.mouseevent.xdata, event.mouseevent.ydata
    x, y = artist.get_xdata(), artist.get_ydata()
    ind = event.ind
    print('Artist picked:', event.artist)
    print('{} vertices picked'.format(len(ind)))
    print('Pick between vertices {} and {}'.format(min(ind), max(ind)+1))
    print('x, y of mouse: {:.2f},{:.2f}'.format(xmouse, ymouse))
    print('Data point:', x[ind[0]], y[ind[0]])
    p2 = [x[ind[0]], 0]
    p1 = [0, 0]
    p0 = [ x[ind[0]], y[ind[0]] ]
    ang = angle (p1, p2, p0) #caluclate angle
    #rotate x, y
    xr = []
    yr = []
    l = len(x)
    print(l)
    anchor = (0, 0)
    for i in range(l):
        v= (x[i], y[i])
        xn=rotate_vector( v, ang, anchor)[0]
        xr.append(xn)  
        yn=rotate_vector( v, ang, anchor)[1]
        yr.append(yn)
    ax.clear()
    ax.plot(xr, yr, 'o')
    plt.draw()


    



class Index(object):
    def height(self, event):
        pass
    def rotate(self, event):
        fig.canvas.callbacks.connect('pick_event', on_pick)
        plt.draw()

 # show an "Open" dialog box and return the path to the selected file
folder = tkFileDialog.askopenfilename() 
f= open ( folder )

xs=[]
ys=[]


#append values to list
for row in csv.reader( f ):
        xs.append( float (row[0] ) )
        ys.append( float (row[1] ) )

    
fig, ax = plt.subplots()
tolerance = 10 # points
ax.plot(xs, ys, 'ro-', picker=tolerance)



callback = Index()
axrotate = plt.axes([0.7, 0.05, 0.1, 0.075])
brotate = Button(axrotate, 'Rotate')
brotate.on_clicked(callback.rotate)
axheight = plt.axes([0.81, 0.05, 0.1, 0.075])
bheight = Button(axheight, 'Distance')  
bheight.on_clicked(callback.height)


plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
