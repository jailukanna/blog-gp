---
title: matplotlib example GForcePlotUsingPySimpleGUI (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'GForcePlotUsingPySimpleGUI'

Functions in program: 
* `def main():`

Modules used in program: 
* `import matplotlib.gridspec as gridspec`
* `import matplotlib.animation as animation`
* `import matplotlib.pyplot as plt`
* `import random, math`
* `import tkinter as tk`
* `import matplotlib.backends.tkagg as tkagg`
* `import PySimpleGUI as sg`
* `import sys`

## python GForcePlotUsingPySimpleGUI

Python matplotlib example: GForcePlotUsingPySimpleGUI

```python
#!/usr/bin/env python
import sys
if sys.version_info[0] >= 3:
    import PySimpleGUI as sg
else:
    import PySimpleGUI27 as sg

from random import randint
import PySimpleGUI as sg
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg, FigureCanvasAgg
from matplotlib.figure import Figure
import matplotlib.backends.tkagg as tkagg
import tkinter as tk

import random, math
import matplotlib.pyplot as plt
import matplotlib.animation as animation
import matplotlib.gridspec as gridspec

class MakeFig:

    def __init__(self):
        self.numOfPoints = 1  # How many current G-force points do we want?
        self.theta = .4  # controls resolution of outer bound points, smaller theta = more points

        # random starter values for global stuff
        self.ActTheta = 0.0
        self.ActRadius = 0.0
        self.points = []
        self.outerPoints = []
        self.barlist = []
        self.maxOverallG = .1
        self.maxSectorG = .1
        self.currentMaxG = .1
        self.fig = plt.figure()
        self.gs = gridspec.GridSpec(2, 1, height_ratios=[4, 1])

        self.ax1 = self.fig.add_subplot(self.gs[0], projection='polar')
        self.ax2 = self.fig.add_subplot(self.gs[1])
        self.initBounds()
        self.initGPoints()
        self.initBars()


    def outerGridCreate(self):
        radius = .5
        p1 = (radius, 0)
        thetaWorking = 0
        while thetaWorking < 2 * math.pi:
            self.outerPoints.append(p1)
            thetaWorking = round((thetaWorking + self.theta), 6)
            p1 = (thetaWorking, radius)

        self.outerPoints.pop(0)  # remove last entry for drawing reasons.

    def randomPoint(self):
        RThet = random.randint(-9, 9) / 20
        RRad = random.randint(-9, 9) / 20
        self.ActTheta = self.ActTheta + RThet
        self.ActRadius = self.ActRadius + RRad

        if self.ActRadius < 0:
            self.ActRadius = abs(self.ActRadius)
        if self.ActRadius > 1.8:
            self.ActRadius = 0 + RRad

        if self.ActTheta < 0:
            self.ActTheta = (2 * math.pi) + self.ActTheta
        if self.ActTheta > 2 * math.pi:
            self.ActTheta = 0 + RThet

        self.ActTheta = round(self.ActTheta, 6)
        self.ActRadius = round(self.ActRadius, 6)

        self.checkOuterBounds(self.ActTheta, self.ActRadius)

        return (self.ActTheta, self.ActRadius)

    def checkOuterBounds(self, pTheta, pRad):

        DistFromCenter = pRad
        self.currentMaxG = DistFromCenter
        if self.currentMaxG == 0:
            self.currentMaxG = .1

        if DistFromCenter < .5:  # we don't need to check these points, so don't bother
            return self.maxOverallG, self.maxSectorG, self.currentMaxG

        # Use theta to figure out what sector we're in
        nearPoint1 = [p for p in self.outerPoints if (p[0] >= pTheta) and (p[0] <= (pTheta + self.theta))]
        nearPoint2 = [x for x in self.outerPoints if (x[0] < pTheta) and (x[0] > (pTheta - self.theta))]

        # If we didn't find anything, it's becuase we're at the zero line sector, so push 'em
        if nearPoint1 == []:
            nearPoint1 = [self.outerPoints[0]]
        if nearPoint2 == []:
            nearPoint2 = [self.outerPoints[-1]]

        nearPoint1 = nearPoint1[0]
        nearPoint2 = nearPoint2[0]

        # Figure out if we need to push the sector boundaries out
        if DistFromCenter > nearPoint1[1]:
            nnp1 = (nearPoint1[0], DistFromCenter)
            self.outerPoints[:] = [nnp1 if (p[0] == nearPoint1[0] and p[1] == nearPoint1[1]) else p for p in self.outerPoints]

        if DistFromCenter > nearPoint2[1]:
            nnp2 = (nearPoint2[0], DistFromCenter)
            self.outerPoints[:] = [nnp2 if (p[0] == nearPoint2[0] and p[1] == nearPoint2[1]) else p for p in self.outerPoints]

        if nearPoint1[1] > nearPoint2[1]:
            self.maxSectorG = nearPoint1[1]
        else:
            self.maxSectorG = nearPoint2[1]

        if self.maxSectorG > self.maxOverallG:
            self.maxOverallG = self.maxSectorG
            self.ax2.axes.set_xlim(0, self.maxOverallG)

    def animateGPoints(self, i):  # animates the actual blue G point reading
        global numOfPoints
        self.points.append(self.randomPoint())
        N = self.numOfPoints
        while len(self.points) > self.numOfPoints:
            self.points.pop(0)
        theta_val = [x[0] for x in self.points]
        rad_val = [x[1] for x in self.points]
        ln = self.ax1.plot(theta_val[-N:], rad_val[-N:], 'bo-')
        return ln

    def animateBounds(self, i):  # animate the outer yellow bound points
        self.ax1.cla()
        self.ax1.set_ylim(0, 2)
        self.ax1.axes.set_yticklabels([])
        self.ax1.axes.set_xticklabels([])

        theta_val = [t[0] for t in self.outerPoints]
        rad_val = [r[1] for r in self.outerPoints]
        theta_val.append(theta_val[0])
        rad_val.append(rad_val[0])
        out = self.ax1.plot(theta_val, rad_val, 'y-')

        return out

    def animateBars(self, i):  # animate the lower limit bars
        Nums = [self.maxOverallG, self.maxSectorG, self.currentMaxG]
        self.ax2.axes.set_ylim(0, 1)
        self.ax2.axes.tick_params(left=False, bottom=False)
        self.ax2.axes.set_yticklabels([])

        for i in enumerate(barlist):
            i[1][0].set_width(Nums[i[0]])
        thing2 = self.ax2.plot()

        self.fig.canvas.draw()

        return thing2

    def initGPoints(self):
        points = []
        c = 0
        while c <= self.numOfPoints:  # create initial list of 10 points
            points.append(self.randomPoint())
            c = c + 1

        ox_val = [x[0] for x in points]  # split it so we can plot it
        oy_val = [x[1] for x in points]
        ln = self.ax1.plot(points[0], points[1])

        return ln

    def initBounds(self):  # generate the initial outside grip circle points
        self.outerGridCreate()
        theta_val = [t[0] for t in self.outerPoints]
        rad_val = [r[1] for r in self.outerPoints]
        out = self.ax1.plot(theta_val, rad_val)

        return out

    def initBars(self):
        global barlist
        max_G = 1.0
        current_sec = .4
        current = .3
        p1 = self.ax2.barh(0, max_G, height=2, color='gray')
        p2 = self.ax2.barh(0, current_sec, height=2, color='red')
        p3 = self.ax2.barh(0, current, height=2, color='green')
        barlist = [p1, p2, p3]
        return barlist



def main():
    # define the form layout
    layout = [[sg.Text('Animated Matplotlib', size=(40, 1), justification='center', font='Helvetica 20')],
              [sg.Canvas(size=(640, 480), key='canvas')],
              [sg.ReadButton('Exit', size=(10, 2), pad=((280, 0), 3), font='Helvetica 14')]]

    # create the form and show it without the plot
    window = sg.Window('Demo Application - Embedding Matplotlib In PySimpleGUI').Layout(layout).Finalize()

    canvas_elem = window.FindElement('canvas')
    canvas = canvas_elem.TKCanvas
    mfig = MakeFig()
    i = 0
    while True:
        event, values = window.Read(timeout=10)
        if event is 'Exit' or event is None:
            exit(69)

        # fig = make_fig()
        fig = mfig.fig
        mfig.animateBars(i)
        mfig.animateBounds(i)
        mfig.animateGPoints(i)
        figure_x, figure_y, figure_w, figure_h = fig.bbox.bounds
        figure_w, figure_h = int(figure_w), int(figure_h)
        photo = tk.PhotoImage(master=canvas, width=figure_w, height=figure_h)

        canvas.create_image(640/2, 480/2, image=photo)

        figure_canvas_agg = FigureCanvasAgg(fig)
        figure_canvas_agg.draw()

        # Unfortunately, there's no accessor for the pointer to the native renderer
        tkagg.blit(photo, figure_canvas_agg.get_renderer()._renderer, colormode=2)

        i += 1


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
