---
title: matplotlib example udacityplots (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'udacityplots'

Functions in program: 
* `def output_image(*args, **kwargs):`
* `def show_plot(width, height=None):`

Modules used in program: 
* `import types `
* `import numpy`
* `import matplotlib, matplotlib.pyplot`
* `import json`
* `import base64`

## python udacityplots

Python matplotlib example: udacityplots

```python
import base64
import json
import matplotlib, matplotlib.pyplot
import numpy
import types 

def show_plot(width, height=None):
    """
    A decorator -- show the matplotlib plot after `f` completes.
    Takes optional parameters (width, height) determining the size of the plot.
    """
    def const_decorator(f):
        def wrapped_f(*args, **kwargs):        
            fig=matplotlib.pyplot.figure(figsize=(width, height))
            ret = f(*args, **kwargs)
            matplotlib.pyplot.show()
            return ret
        return wrapped_f
    if type(width)==types.FunctionType:
        f = width
        width, height = 9, 4
        return const_decorator(f)
    else:
        return const_decorator
        

def output_image(*args, **kwargs):
    raise NotImplementedError

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
