---
title: matplotlib example wxpython-serialdata-matplotlib (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'wxpython-serialdata-matplotlib'


Modules used in program: 
* `import matplotlib`
* `import serial`
* `import wx`

## python wxpython-serialdata-matplotlib

Python matplotlib example: wxpython-serialdata-matplotlib

```python
#!/usr/local/bin/python
# -*- coding: utf-8 -*-

import wx
import serial

# Import matplotlib for wxPython
import matplotlib
matplotlib.use('WXAgg')

from matplotlib.backends.backend_wxagg import FigureCanvasWxAgg as FigureCanvas
from matplotlib.backends.backend_wx import NavigationToolbar2Wx
from matplotlib.figure import Figure


class MyFrame(wx.Frame):
    def __init__(self, parent, title):
        super(MyFrame, self).__init__(parent, title=title,
            size=(350, 250))

        # Initialize the Matplotlib graph
        self.figure = Figure()
        self.axes = self.figure.add_subplot(111)
        self.canvas = FigureCanvas(self, -1, self.figure)

        # Create a timer for redrawing the frame every 100 milliseconds
        self.Timer = wx.Timer(self)
        self.Timer.Start(100)
        self.Bind(wx.EVT_TIMER, self.OnPaint)

        # Show the frame
        self.Centre()
        self.Show()


    def OnPaint(self, event=None):
        # Get data from serial port
        value = arduino.readline()

        # Draw the serial data

        # Cleare the previos graph
        self.axes.clear()

        # Set the Y axis limit in order to provide consistency in the visualization
        self.axes.set_ylim([0,100])

        # Draw the bar
        self.axes.bar(1,value)

        # Update the graph
        self.canvas.draw()


# Main program
if __name__ == '__main__':
    # Connect to serial port first
    try:
        arduino = serial.Serial('/dev/tty.usbmodem1421', 9600)
    except:
        print("Failed to connect")
        exit()
 
    # Create and launch the wx interface
    app = wx.App()
    MyFrame(None, 'Serial data test')
    app.MainLoop()
 
    # Close the serial connection
    arduino.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
