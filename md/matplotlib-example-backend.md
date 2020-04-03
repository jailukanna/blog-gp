---
title: matplotlib example backend (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'backend'

Functions in program: 
* `def display(fig):`

Modules used in program: 
* `import IPython`
* `import base64`
* `import base64`
* `import io`

## python backend

Python matplotlib example: backend

```python
# import matplotlib                                                                                                                                                                                   
# matplotlib.use('module://python.my_backend')

import io
import base64

from matplotlib.backend_bases import (
    _Backend, FigureCanvasBase, FigureManagerBase, RendererBase)

from matplotlib.backends.backend_agg import RendererAgg, FigureCanvasAgg, FigureManagerBase
from matplotlib._pylab_helpers import Gcf
import base64
import IPython

class MyFigureCanvas(FigureCanvasAgg):
    def draw(self, *args, **kwargs):
        return 100

class MyFigureManager(FigureManagerBase):
    pass

def display(fig):
    png = base64.encodebytes(IPython.core.pylabtools.print_figure(fig))   
    tag = f'<img src="data:image/png;base64,{png}">'
    print(tag)

@_Backend.export
class _BackendAgg(_Backend):
    FigureCanvas = MyFigureCanvas
    FigureManager = MyFigureManager

    def show(close=None, block=None):
        for figure_manager in Gcf.get_all_fig_managers():
            display(figure_manager.canvas.figure)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
