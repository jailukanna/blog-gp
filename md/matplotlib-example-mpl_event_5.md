---
title: matplotlib example mpl event 5 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mpl event 5'

Functions in program: 
* `def onpick(event):`

Modules used in program: 
* `import matplotlib`
* `import ipywidgets`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python mpl event 5

Python matplotlib example: mpl event 5

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
plt.rcParams['font.size']=12
plt.rcParams['font.family'] = 'sans-serif'


fig,ax = plt.subplots(figsize=(6,4))
ax.set_title('click on points')

points, = ax.plot(10*np.random.rand(30),10*np.random.rand(30), 'C2o',mew=2,mec='k', ms=15,picker=10)  # 10 points tolerance

out = Output()
display(out)

@out.capture(clear_output=True)
def onpick(event):
    thisline = event.artist
    xdata = thisline.get_xdata()
    ydata = thisline.get_ydata()
    ind = event.ind
    ax.plot(xdata[ind], ydata[ind],"C6o",ms=12)
    points = tuple(zip(xdata[ind], ydata[ind]))
    print('onpick points:', points)
    fig.canvas.draw()

fig.canvas.mpl_connect('pick_event', onpick)
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
