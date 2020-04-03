---
title: matplotlib example pyqt-mpl (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'pyqt-mpl'

Functions in program: 
* `def createSlider():`
* `def simulate(R0, J0, a, b, c, d, dt=1e-3, N=1000):`

Modules used in program: 
* `import matplotlib`
* `import numpy as np`
* `import sys`

## python pyqt-mpl

Python matplotlib example: pyqt-mpl

```python
import sys
import numpy as np

import matplotlib

matplotlib.use('Qt5Agg')

from PyQt5 import QtWidgets
from PyQt5.QtCore import Qt
from PyQt5.QtWidgets import QApplication, QDialog, QWidget
from PyQt5.QtWidgets import QGridLayout, QVBoxLayout, QFormLayout
from PyQt5.QtWidgets import QSlider, QLabel, QLineEdit

from matplotlib.backends.backend_qt5agg import FigureCanvasQTAgg
from matplotlib.figure import Figure
from matplotlib import pyplot as plt

from collections import namedtuple

Params = namedtuple('Params', ['R0', 'J0', 'a', 'b', 'c', 'd', 'dt', 'N'])


def simulate(R0, J0, a, b, c, d, dt=1e-3, N=1000):
  x = np.zeros((N, 2))
  x[0][0] = R0
  x[0][1] = J0

  A = np.array([[a, b], [c, d]])

  for i in range(1, N):
    x[i] = x[i - 1] + A @ x[i - 1] * dt

  return x.T


def createSlider():
  slider = QSlider(Qt.Horizontal)
  slider.setTickInterval(1)
  slider.setSingleStep(1e-3)

  return slider


class Controls(QWidget):

  def __init__(self, parent=None):
    super().__init__(parent)

    self.form = QFormLayout()
    self.form.addRow(QLabel('N'), QLineEdit())
    self.form.addRow(QLabel('a'), createSlider())
    self.form.addRow(QLabel('b'), createSlider())
    self.form.addRow(QLabel('c'), createSlider())
    self.form.addRow(QLabel('d'), createSlider())
    self.form.addRow(QLabel('Δt'), QLineEdit())

    self.setLayout(self.form)


class MplCanvas(FigureCanvasQTAgg):

  def __init__(self, parent=None):
    self.figure = Figure(facecolor='#eeeeee')

    FigureCanvasQTAgg.__init__(self, self.figure)

    self.setParent(parent)

    FigureCanvasQTAgg.setSizePolicy(self, QtWidgets.QSizePolicy.Expanding,
                                    QtWidgets.QSizePolicy.Expanding)
    FigureCanvasQTAgg.updateGeometry(self)

  def render(self):
    pass


class PhaseSpace(MplCanvas):

  def render(self):
    R0 = 1
    J0 = 0

    a = 1
    b = -1
    c = -1
    d = -1

    R, J = simulate(R0, J0, a, b, c, d)

    self.figure.clear()

    ax = self.figure.add_subplot(111)
    ax.scatter(R, J)

    self.draw()


class Window(QDialog):

  def __init__(self, parent=None):
    super().__init__(parent)

    self.canvas1 = PhaseSpace()
    self.canvas2 = PhaseSpace()
    self.canvas3 = PhaseSpace()
    self.controls = Controls()

    self.layout = QGridLayout()
    self.layout.addWidget(self.canvas1, 0, 0, 2, 2)
    self.layout.addWidget(self.canvas2, 2, 0, 1, 1)
    self.layout.addWidget(self.canvas3, 2, 1, 1, 1)
    self.layout.addWidget(self.controls, 0, 2, 3, 2)

    self.setWindowTitle('Complex systems')
    self.setLayout(self.layout)

    self.canvas1.render()
    self.canvas2.render()
    self.canvas3.render()


if __name__ == '__main__':
  app = QApplication(sys.argv)

  main = Window()
  main.show()

  sys.exit(app.exec_())


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
