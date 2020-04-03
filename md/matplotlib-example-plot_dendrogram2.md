---
title: matplotlib example plot dendrogram2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot dendrogram2'


Modules used in program: 
* `import numpy as np`
* `import matplotlib`
* `import sys`

## python plot dendrogram2

Python matplotlib example: plot dendrogram2

```python
from scipy.cluster.hierarchy import dendrogram, linkage
import sys
import matplotlib
matplotlib.use("Qt5Agg")
import numpy as np
from numpy import arange, sin, pi
from matplotlib.backends.backend_qt5agg import NavigationToolbar2QT as NavigationToolbar
from matplotlib.backends.backend_qt5agg import FigureCanvasQTAgg as FigureCanvas
from matplotlib.figure import Figure
from matplotlib import pyplot as plt
from PyQt5.QtWidgets import QWidget, QMainWindow, QApplication, QSizePolicy, QVBoxLayout, QPushButton
from PyQt5.QtCore import *
from PyQt5.QtGui import QCursor

class MyMplCanvas(FigureCanvas):
    #Ultimately, this is a QWidget (as well as a FigureCanvasAgg, etc.).
    def __init__(self, parent=None, width=5, height=4, dpi=100):
        fig = Figure(figsize=(width, height), dpi=dpi)
        self.fig = fig
        self.axes = fig.add_subplot(111)
        # We want the axes not cleared every time plot() is called
        self.axes.hold(True)

        self.compute_initial_figure()
        super(MyMplCanvas, self).__init__(fig)
        self.setParent(parent)

        self.setSizePolicy(QSizePolicy.Expanding, QSizePolicy.Expanding)
        self.updateGeometry()

    def compute_initial_figure(self):
        pass


class MyStaticMplCanvas(MyMplCanvas):
    #Simple canvas with a sine plot.
    def compute_initial_figure(self):
        #t = arange(0.0, 3.0, 0.01)
        #s = sin(2*pi*t)
        #self.axes.plot(t, s)

        self.createSampleDate()

        # generate the linkage matrix
        Z = linkage(self.X, 'ward')

        ddata = self.fancy_dendrogram(
          Z,
          leaf_rotation=90., # rotates the x axis labels
          leaf_font_size=8., # font size for the x axis labels
          #truncate_mode='lastp',  # show only the last p merged clusters
          #p=12,  # show only the last p merged clusters
          #show_contracted=True,  # to get a distribution impression in truncated branches
        )

    def createSampleDate(self):
      # generate two clusters: a with 100 points, b with 50:
      np.random.seed(4711)  # for repeatability of this tutorial
      a = np.random.multivariate_normal([10, 0], [[3, 1], [1, 4]], size=[100,])
      b = np.random.multivariate_normal([0, 20], [[3, 1], [1, 4]], size=[50,])
      self.X = np.concatenate((a, b),)

    def fancy_dendrogram(self, *args, **kwargs):
          max_d = kwargs.pop('max_d', None)
          if max_d and 'color_threshold' not in kwargs:
              kwargs['color_threshold'] = max_d
          annotate_above = kwargs.pop('annotate_above', 0)

          ddata = dendrogram(*args, **kwargs)

          if not kwargs.get('no_plot', False):
              #self.axes.set_title('Hierarchical Clustering Dendrogram (truncated)')
              #self.axes.set_xlabel('sample index or (cluster size)')
              #self.axes.set_ylabel('distance')

              for i, d, c in zip(ddata['icoord'], ddata['dcoord'], ddata['color_list']):
                  x = 0.5 * sum(i[1:3])
                  y = d[1]
                  if y > annotate_above:
                      self.axes.plot(x, y, 'o', c=c)
                      self.axes.annotate("%.3g" % y, (x, y), xytext=(0, -5),
                                   textcoords='offset points',
                                   va='top', ha='center')
              if max_d:
                  self.axes.axhline(y=max_d, c='k', linestyle='--', color='red')

          #self.axes.set_ylim(ymin=0, ymax=500)
          #self.axes.set_xlim(xmin=0, xmax=1000)

          return ddata

class PlotDialog(QWidget):
    def __init__(self):
        QWidget.__init__(self)

        self.plot_layout = QVBoxLayout(self)
        self.plot_canvas = MyStaticMplCanvas(self, width=5, height=4, dpi=100)

        self.navi_toolbar = NavigationToolbar(self.plot_canvas, self)
        self.plot_layout.addWidget(self.plot_canvas)  # the matplotlib canvas
        self.plot_layout.addWidget(self.navi_toolbar)

        self.setLayout(self.plot_layout)

if __name__ == "__main__":
    import sys
    app = QApplication(sys.argv)
    dialog = PlotDialog()
    dialog.show()
    sys.exit(app.exec_())

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
