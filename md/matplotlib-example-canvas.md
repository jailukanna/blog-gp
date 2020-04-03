---
title: matplotlib example canvas (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'canvas'


Modules used in program: 
* `import matplotlib.pyplot as plt`

## python canvas

Python matplotlib example: canvas

```python
from matplotlib.backends.backend_qt4agg import FigureCanvasQTAgg as FigureCanvas 
import matplotlib.pyplot as plt

class Canvas(FigureCanvas):
    def __init__(self, parent=None):
        self.figure = plt.figure()
        FigureCanvas.__init__(self, self.figure)
        self.setParent(parent)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
