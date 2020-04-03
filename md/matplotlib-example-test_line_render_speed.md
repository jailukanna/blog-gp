---
title: matplotlib example test line render speed (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'test line render speed'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib as mpl`
* `import numpy as np`
* `import sys`

## python test line render speed

Python matplotlib example: test line render speed

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
""" test *big* Line rendering performance
in relation to issue #1256
https://github.com/matplotlib/matplotlib/issues/1256

Pierre Haessig — October 2012
"""

from timeit import repeat
import sys
# Add path to local development version of matplotlib:
sys.path.insert(1, '/home/pierre/.local/lib/python2.7/site-packages')

import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt


print('Line rendering performance test')
print('matplotlib version: %s' % mpl.__version__)

# Creates big x and y data:
N = 10**7
print('generating big data vectors (N=10^%.1f)...' % np.log10(N))
t = np.linspace(0,1,N)
x = np.random.normal(size=N)

# Create a plot:
fig = plt.figure()
ax = plt.subplot(111)

# Create a "big" Line instance:
l = mpl.lines.Line2D(t,x)
l.set_visible(False)
# but don't add it to the Axis instance `ax`

# [here Interactive panning and zooming is pretty responsive]
# Time the canvas drawing
t_no_line = min(repeat(fig.canvas.draw, number=1, repeat=3))
print('canvas drawing without the big Line: %.1f ms' % (t_no_line *1000) )
# gives about 25 ms

# Add the big invisible Line:
ax.add_line(l)

# [Now interactive panning and zooming is very slowdowned]
# Time the canvas drawing
t_unvisible_line = min(repeat(fig.canvas.draw, number=1, repeat=3))
print('canvas drawing with the big invisible Line: %.1f ms' % (t_unvisible_line *1000) )
# gives about 290 ms for N = 10**7 pts (50 ms for 10**6)

is_bug = (t_unvisible_line/t_no_line) > 2
print('Is there the big line rendering bug ? -> %s' % ('YES' if is_bug else 'NO'))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
