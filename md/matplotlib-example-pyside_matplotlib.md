---
title: matplotlib example pyside matplotlib (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'pyside matplotlib'


Modules used in program: 
* `import matplotlib`
* `import sys`

## python pyside matplotlib

Python matplotlib example: pyside matplotlib

```python
#!/usr/bin/env python
# http://wiki.scipy.org/Cookbook/Matplotlib/PySide
import sys
import matplotlib

matplotlib.use('Qt4Agg')
matplotlib.rcParams['backend.qt4']='PySide'

from matplotlib.backends.backend_qt4agg import FigureCanvasQTAgg as FigureCanvas
from matplotlib.figure import Figure

from PySide import QtCore, QtGui

if __name__ == '__main__':
    app = QtGui.QApplication(sys.argv)

    # generate plot
    fig = Figure(figsize=(600, 600), dpi=72, facecolor=(1,1,1), edgecolor=(0,0,0))
    ax = fig.add_subplot(111)
    ax.plot([0,1])
    # generate the canvas to display the plot
    canvas = FigureCanvas(fig)

    win = QtGui.QMainWindow()
    # add the plot canvas to a window
    win.setCentralWidget(canvas)

    win.show()

    sys.exit(app.exec_())

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
