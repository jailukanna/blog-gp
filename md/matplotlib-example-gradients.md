---
title: matplotlib example gradients (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'gradients'

Functions in program: 
* `def gradient_hsv_custom(v):`
* `def gradient_hsv_unknown(v):`
* `def gradient_hsv_bgr(v):`
* `def gradient_hsv_bw(v):`
* `def gradient_rgb_wb_custom(v):`
* `def gradient_rgb_gbr_full(v):   #do poprawy`
* `def gradient_rgb_gbr(v):`
* `def gradient_rgb_bw(v):`
* `def plot_color_gradients(gradients, names):`

Modules used in program: 
* `import math as m`
* `import colorsys`
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib`

## python gradients

Python matplotlib example: gradients

```python

# -*- coding: utf-8 -*-
from __future__ import division             # Division in Python 2.7
import matplotlib
#matplotlib.use('Agg')                       # So that we can render files without GUI
import matplotlib.pyplot as plt
from matplotlib import rc
import numpy as np

from matplotlib import colors
import colorsys
import math as m

def plot_color_gradients(gradients, names):
    # For pretty latex fonts
    #rc('text', usetex=True)
    #rc('font', family='serif', serif=['Times'], size=10)
    #rc('legend', fontsize=10)

    column_width_pt = 400         # Show in latex using \the\linewidth
    pt_per_inch = 72
    size = column_width_pt / pt_per_inch

    fig, axes = plt.subplots(nrows=len(gradients), sharex=True, figsize=(size, 0.75 * size))
    fig.subplots_adjust(top=1.00, bottom=0.05, left=0.25, right=0.95)


    for ax, gradient, name in zip(axes, gradients, names):
        # Create image with two lines and draw gradient on it
        img = np.zeros((2, 1024, 3))
        for i, v in enumerate(np.linspace(0, 1, 1024)):
            img[:, i] = gradient(v)

        im = ax.imshow(img, aspect='auto')
        im.set_extent([0, 1, 0, 1])
        ax.yaxis.set_visible(False)

        pos = list(ax.get_position().bounds)
        x_text = pos[0] - 0.25
        y_text = pos[1] + pos[3]/2.
        fig.text(x_text, y_text, name, va='center', ha='left', fontsize=10)

    fig.savefig('my-gradients.pdf')

def gradient_rgb_bw(v):
    
    return (v, v, v)


def gradient_rgb_gbr(v):
    r, g, b = 0, 0, 0

    if v >= 0.5:
        r = 2 * v - 1
        b = 2 * (-v + 1) 

    else:
        g = -2* v + 1
        b = 2 * v
    
    return (r, g, b)   


def gradient_rgb_gbr_full(v):   #do poprawy
    r, g, b = 0, 0, 0

    if v <= 0.25:
        g = 1
        b = 4 * v

    if v > 0.25 and v <= 0.5:
        b = 1
        g = -4 * v + 2

    if v > 0.5 and v <= 0.75:
        b = 1
        r = 4*v - 2

    if v > 0.75 and v <= 1:
        r = 1
        b = -4 * v + 4        

    return (r, g, b)


def gradient_rgb_wb_custom(v):
    r,g,b = 0,0,0

    if v <= 1/7:
        r, b = 1, 1
        g = -7 * v

    if v > 1/7 and v <= 2/7:
        b = 1
        r = -7 * v + 2

    if v > 2/7 and v <= 3/7:
        b = 1
        g = 7 * v - 2

    if v > 3/7 and v <= 4/7:
        g = 1
        b = -7 * v + 4

    if v > 4/7 and v <= 5/7:
        g = 1
        r = 7 * v - 4

    if v > 5/7 and v <= 6/7:
        g = -7 * v + 6
        r = 1

    if v > 6/7:
        r = -7 * v + 7
    
    return (r, g, b)


def gradient_hsv_bw(v):
    return colorsys.hsv_to_rgb(0, 0, v)


def gradient_hsv_bgr(v):
    return colorsys.hsv_to_rgb(2/3*v+1/3, 1, 1)


def gradient_hsv_unknown(v):
    return colorsys.hsv_to_rgb((-v+1)/3, 0.5, 1)


def gradient_hsv_custom(v):
    return colorsys.hsv_to_rgb(v, -v+1, 1)


if __name__ == '__main__':
    def toname(g):
        return g.__name__.replace('gradient_', '').replace('_', '-').upper()

    gradients = (gradient_rgb_bw, gradient_rgb_gbr, gradient_rgb_gbr_full, gradient_rgb_wb_custom,
                 gradient_hsv_bw, gradient_hsv_bgr, gradient_hsv_unknown, gradient_hsv_custom)

    plot_color_gradients(gradients, [toname(g) for g in gradients])

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
