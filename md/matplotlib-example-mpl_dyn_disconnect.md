---
title: matplotlib example mpl dyn disconnect (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mpl dyn disconnect'


Modules used in program: 
* `import matplotlib.widgets as widgets`
* `import matplotlib`

## python mpl dyn disconnect

Python matplotlib example: mpl dyn disconnect

```python
from PySide import QtCore, QtGui
import matplotlib
import matplotlib.widgets as widgets
matplotlib.use('Qt4Agg')
from matplotlib.backends.backend_qt4agg import FigureCanvasQTAgg as FigureCanvas
from matplotlib.figure import Figure

class GraphWindow(QtGui.QMainWindow):
    def __init__(self, parent=None):
        super(GraphWindow, self).__init__(parent)

        self.tb = QtGui.QToolBar()
        self.act1 = self.tb.addAction('h')
        self.act1.setCheckable(True)
        self.act1.setChecked(True)
        self.act2 = self.tb.addAction('v')
        self.act2.setCheckable(True)
        self.group = QtGui.QActionGroup(self)
        self.group.addAction(self.act1)
        self.group.addAction(self.act2)

        self.addToolBar(self.tb)

        self.figure = Figure()
        self.canvas = FigureCanvas(self.figure)
        self.ax = self.figure.add_subplot(1, 1, 1)
        self.canvas.mpl_connect('button_press_event', self.on_press)

        self.active_tool = None

        self.setCentralWidget(self.canvas)

    def on_press(self, event):
        if self.active_tool != None:
            self.active_tool.disconnect_events()

        if self.group.checkedAction() == self.act1:
            self.active_tool = widgets.SpanSelector(self.ax, self.on_done, 'horizontal')
        if self.group.checkedAction() == self.act2:
            self.active_tool = widgets.SpanSelector(self.ax, self.on_done, 'vertical')
        self.active_tool.press(event)

    def on_done(self, *args):
        print(args)

if __name__ == '__main__':
    app = QtGui.QApplication([])
    m = GraphWindow()
    m.show()
    app.exec_()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
