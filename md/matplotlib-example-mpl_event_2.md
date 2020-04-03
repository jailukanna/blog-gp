---
title: matplotlib example mpl event 2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mpl event 2'


Modules used in program: 
* `import matplotlib`
* `import ipywidgets`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python mpl event 2

Python matplotlib example: mpl event 2

```python
#jupyterlab
%matplotlib widget
#jupyter notebook
#%matplotlib notebook
import numpy as np
import matplotlib.pyplot as plt
from ipywidgets import Output
plt.rcParams['font.size']=12
plt.rcParams['font.family'] = 'sans-serif'
out = Output()
display(out)

class LineBuilder:
    def __init__(self, line):
        self.line = line
        self.xs = list(line.get_xdata())
        self.ys = list(line.get_ydata())
        self.cid = line.figure.canvas.mpl_connect('button_release_event', self)
    
    @out.capture(clear_output=True)       
    
    def __call__(self, event):  
        print('click', (event.xdata,event.ydata))
        if event.inaxes!=self.line.axes: return
        self.xs.append(event.xdata)
        self.ys.append(event.ydata)
        self.line.set_data(self.xs, self.ys)
        self.line.figure.canvas.draw()

fig = plt.figure(figsize=(4,4))
ax = fig.add_subplot(111)
ax.set_title('click to build line segments')
line, = ax.plot([0], [0],"C2o--")  # empty line
linebuilder = LineBuilder(line)
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
