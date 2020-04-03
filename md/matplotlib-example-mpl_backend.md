---
title: matplotlib example mpl backend (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mpl backend'

Functions in program: 
* `def new_figure_manager(num, *args, **kwargs):`
* `def new_figure_manager_given_figure(num, figure):`
* `def draw_if_interactive():`
* `def show(self, close=None, block=None, **kwargs):`

Modules used in program: 
* `import matplotlib`
* `import base64`
* `import warnings`
* `import uuid`
* `import sys`

## python mpl backend

Python matplotlib example: mpl backend

```python
# Adapted from Zeppelin's backend
# https://raw.githubusercontent.com/apache/zeppelin/1972a58620b16bc4ec54191fb8660cf274647bd8/interpreter/lib/python/backend_zinline.py
# Zeppelin is licensed under the Apache License, Version 2.0
#    http://www.apache.org/licenses/LICENSE-2.0

from __future__ import print_function

import sys
import uuid
import warnings
import base64
from io import BytesIO
try:
    from StringIO import StringIO
except ImportError:
    from io import StringIO

import matplotlib
from matplotlib._pylab_helpers import Gcf
from matplotlib.backends.backend_agg import FigureCanvasAgg
from matplotlib.backend_bases import FigureManagerBase
from matplotlib.figure import Figure


def show(self, close=None, block=None, **kwargs):
    try:
        managers = Gcf.get_all_fig_managers()

        # Show all open figures
        for manager in managers:
            manager.show(**kwargs)
    finally:
        # This closes all the figures if close is set to True.
        if close and Gcf.get_all_fig_managers():
            Gcf.destroy_all()


def draw_if_interactive():
    manager = Gcf.get_active()
    interactive = matplotlib.is_interactive()
    fig = manager.canvas.figure
    fig.show = show


class FigureManager(FigureManagerBase):
    def show(self, **kwargs):
        out = BytesIO()
        self.canvas.print_png(out)
        print(out)


def new_figure_manager_given_figure(num, figure):
    canvas = FigureCanvasAgg(figure)
    manager = FigureManager(canvas, num)
    return manager


def new_figure_manager(num, *args, **kwargs):
    fig_cls = kwargs.pop('FigureClass', Figure)
    fig = fig_cls(*args, **kwargs)
    return new_figure_manager_given_figure(num, fig)


FigureCanvas = FigureCanvasAgg


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
