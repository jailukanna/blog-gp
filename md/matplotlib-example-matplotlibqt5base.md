---
title: matplotlib example matplotlibqt5base (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlibqt5base'

Functions in program: 
* `def main():`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import sys`

## python matplotlibqt5base

Python matplotlib example: matplotlibqt5base

```python
import sys
import numpy as np
import matplotlib.pyplot as plt
# import relevant classes from PyQt
from PyQt5.QtWidgets import QApplication, QMainWindow, QPushButton, QWidget, QLabel
from PyQt5.QtWidgets import QHBoxLayout, QVBoxLayout, QSizePolicy
from matplotlib.backends.backend_qt5agg import FigureCanvasQTAgg as FigureCanvas


class MyMainWindow(QMainWindow):
    ''' the main window potentially with menus, statusbar, ... '''

    def __init__(self):
        super().__init__()
        self.resize(600, 300)
        self.move(300, 200)
        central_widget = MyCentralWidget(self)
        self.setCentralWidget(central_widget)
        self.setWindowTitle('PyQt MainWindow with matplotlib figure')
        self.statusBar().showMessage('Waiting for a button to be pressed')


class MyCentralWidget(QWidget):
    ''' everything in the main area of the main window '''

    def __init__(self, main_window):
        super().__init__()
        self.main_window = main_window
        # define left button
        left_button = QPushButton('Plot x^2', self)
        left_button.clicked.connect(self.on_left_button_clicked)    # <-- registers which function to call when the button is clicked
        # define right button
        right_button = QPushButton('Plot x^3', self)
        right_button.clicked.connect(self.on_right_button_clicked)
        # define figure canvas
        self.mpl_widget = MyMplWidget()                             # <-- create the matplotlib figure widget (class defined below)
        # place the buttons into a horizontal box layout
        hbox = QHBoxLayout()
        hbox.addStretch(1)                                          # <-- add an expanding box 
        hbox.addWidget(left_button)                                 # <-- add the left button
        hbox.addWidget(right_button)                                # <-- add the right button
        # place this box and the label into a vertical box layout
        vbox = QVBoxLayout()
        vbox.addWidget(self.mpl_widget)                             # <-- add the figure 
        vbox.addLayout(hbox)                                        # <-- add the hbox with the buttons
        #use the box layout to fill the window
        self.setLayout(vbox) 

    def on_left_button_clicked(self):
        ''' event handler for click on left button '''
        self.main_window.statusBar().showMessage('Plot of x^2')
        self.mpl_widget.plot_power(2)

    def on_right_button_clicked(self):
        ''' event handler for click on right button '''
        self.main_window.statusBar().showMessage('Plot of x^3')
        self.mpl_widget.plot_power(3)

 
class MyMplWidget(FigureCanvas):
    ''' both a QWidget and a matplotlib figure '''

    def __init__(self, parent=None, figsize=(4, 3), dpi=100):
        self.fig = plt.figure(figsize=figsize, dpi=dpi) # creates a matplotlib figure
        FigureCanvas.__init__(self, self.fig) # initialises the FigureCanvas object with the figure
        self.setParent(parent)
        # ensure figure expands with window
        FigureCanvas.setSizePolicy(self, QSizePolicy.Expanding, QSizePolicy.Expanding) 
        FigureCanvas.updateGeometry(self)

    def plot_power(self, n):
        ''' clear the figure and plot x**n for -1 < x < 1 '''
        self.fig.clf()                                              # <-- clears the figure 
        self.ax = self.fig.add_subplot(1, 1, 1)
        x = np.linspace(-1, 1, 101)
        self.ax.plot(x, x**n, 'b')
        self.draw()


app = None

def main():
    global app
    app = QApplication(sys.argv)
    w = MyMainWindow()
    w.show()
    app.exec()

if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
