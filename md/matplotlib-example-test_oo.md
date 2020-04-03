---
title: matplotlib example test oo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'test oo'


Modules used in program: 
* `import guppy`
* `import numpy as np`
* `import matplotlib as mpl`

## python test oo

Python matplotlib example: test oo

```python
# Object-oriented API
#
# Memory usage (iteration, object count, memory size)
# 100 5637 1562216
# 200 5529 1491528
# 300 5422 1426264
# 400 5758 1587376
# 500 5422 1426288
# 600 5416 1440456
# 700 5610 1515056
# 800 5422 1426032
# 900 5530 1493264
# 
# 241.35 real       239.52 user         1.38 sys

import matplotlib as mpl
mpl.use('Agg')

from matplotlib.figure import Figure
from matplotlib.backends.backend_agg import FigureCanvasAgg as FigureCanvas

import numpy as np

import guppy

heapy = guppy.hpy()

mem = open('memory_oo.txt', 'wb')

heapy.setref()

for i in range(1000):

    if i % 100 == 0:
        h = heapy.heap()
        mem.write('%i %s %s\n' % (i, h.count, h.size))

    fig = Figure()
    canvas = FigureCanvas(fig)
    ax = fig.add_subplot(1, 1, 1)
    ax.scatter(np.random.random(10), np.random.random(10))
    canvas.print_figure('test.png')

mem.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
