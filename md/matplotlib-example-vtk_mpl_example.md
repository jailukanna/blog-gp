---
title: matplotlib example vtk mpl example (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'vtk mpl example'


Modules used in program: 
* `import vtk`
* `import sys`
* `import numpy as np`
* `import IPython`
* `import sys`

## python vtk mpl example

Python matplotlib example: vtk mpl example

```python
from __future__ import print_function

import sys

import IPython
ip = IPython.get_ipython()
ip.run_line_magic("matplotlib", "qt5")

import numpy as np
from matplotlib.figure import Figure
from matplotlib.backend_bases import key_press_handler
from matplotlib.backends.backend_qt5agg import (
    FigureCanvasQTAgg as FigureCanvas,
    NavigationToolbar2QT as NavigationToolbar)
#from matplotlib.backends import qt5_compat
# use_pyside = qt5_compat.QT_API == qt5_compat.QT_API_PYSIDE
use_pyside = False
import sys
import vtk
from PyQt5 import QtCore, QtWidgets
from vtk.qt.QVTKRenderWindowInteractor import QVTKRenderWindowInteractor

from PyQt5.QtCore import *
from PyQt5.QtWidgets import *


class AppForm(QMainWindow):
    def __init__(self, parent=None):
        QMainWindow.__init__(self, parent)
        #self.x, self.y = self.get_data()
        self.data = self.get_data2()
        self.create_main_frame()
        self.on_draw()

    def create_main_frame(self):
        self.main_frame = QWidget()

        self.fig = Figure((5.0, 4.0), dpi=100)
        self.canvas = FigureCanvas(self.fig)
        self.canvas.setParent(self.main_frame)
        self.canvas.setFocusPolicy(Qt.StrongFocus)
        self.canvas.setFocus()

        self.mpl_toolbar = NavigationToolbar(self.canvas, self.main_frame)

        self.canvas.mpl_connect('key_press_event', self.on_key_press)

        vbox = QVBoxLayout()
        vbox.addWidget(self.canvas)  # the matplotlib canvas
        vbox.addWidget(self.mpl_toolbar)


        # make the vtk widget
        self.frame = QtWidgets.QFrame()
        self.vl = QtWidgets.QVBoxLayout()
        self.vtkWidget = QVTKRenderWindowInteractor(self.frame)
        vbox.addWidget(self.vtkWidget)

        self.ren = vtk.vtkRenderer()
        self.vtkWidget.GetRenderWindow().AddRenderer(self.ren)
        self.iren = self.vtkWidget.GetRenderWindow().GetInteractor()


        # Create source
        source = vtk.vtkCylinderSource()
        source.SetCenter(0, 0, 0)
        source.SetRadius(5.0)
        source.SetHeight(5.0)
        self.source = source

        # Filter the object through a rotation
        theta = np.radians(23)
        rotmat = np.array([[np.cos(theta), np.sin(theta), 0, 0],
                           [-np.sin(theta), np.cos(theta), 0, 0],
                           [0, 0, 1, 0],
                           [0, 0, 0, 1],
                          ])

        trans = vtk.vtkTransform()
        trans.SetMatrix(rotmat.ravel())

        filt1 = vtk.vtkTransformFilter()
        filt1.SetInputConnection(source.GetOutputPort())
        filt1.SetTransform(trans)

        # Create a mapper
        mapper = vtk.vtkPolyDataMapper()
        mapper.SetInputConnection(filt1.GetOutputPort())
        self.mapper = mapper


        # Create an actor
        actor = vtk.vtkActor()
        actor.SetMapper(mapper)

        self.ren.AddActor(actor)

        self.ren.ResetCamera()


        self.main_frame.setLayout(vbox)
        self.setCentralWidget(self.main_frame)

        self.show()
        self.iren.Initialize()

    def get_data2(self):
        x = np.linspace(-10, 10, 100)
        X,Y = np.meshgrid(x,x)
        R = np.hypot(X,Y)
        return R < 1

    def on_draw(self):
        self.fig.clear()
        self.axes = self.fig.add_subplot(111)
        #self.axes.plot(self.x, self.y, 'ro')
        self.axes.imshow(self.data, interpolation='nearest')
        #self.axes.plot([1,2,3])
        self.canvas.draw()

    def on_key_press(self, event):
        print('you pressed', event.key)
        # implement the default mpl key press events described at
        # http://matplotlib.org/users/navigation_toolbar.html#navigation-keyboard-shortcuts
        key_press_handler(event, self.canvas, self.mpl_toolbar)



app = QApplication(sys.argv)
form = AppForm()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
