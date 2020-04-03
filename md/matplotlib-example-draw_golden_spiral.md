---
title: matplotlib example draw golden spiral (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'draw golden spiral'

Functions in program: 
* `def label(xy, text, j):`
* `def fib(n):`

Modules used in program: 
* `import numpy`
* `import matplotlib.patches as mpatches`
* `import matplotlib.lines as mlines`
* `import matplotlib.path as mpath`
* `import matplotlib.pyplot as plt`
* `import matplotlib.pyplot as plt`

## python draw golden spiral

Python matplotlib example: draw golden spiral

```python
"""
Draws Golden Spriral

Adapted from
http://junilyd.github.io/blog/2014/08/13/fibonacci-mystery-pythonified/

Tuomas Karna
2016-05-14
"""
import matplotlib.pyplot as plt
plt.rcdefaults()
import matplotlib.pyplot as plt
import matplotlib.path as mpath
import matplotlib.lines as mlines
import matplotlib.patches as mpatches
from matplotlib.collections import PatchCollection
import numpy


def fib(n):
    if n < 2:
        return n
    else:
        return fib(n - 1) + fib(n - 2)


def label(xy, text, j):
    y = xy[1] - 0.025  # shift y-value to center label
    plt.text(xy[0], y, text, ha="center", family='sans-serif', size=j)

fig, ax = plt.subplots()
patches = []
lw = 3.0

xy = numpy.array([0, 0])
c = xy
i = 1
t = numpy.array([270, 0])
for j in range(2, 8):
    t += 90
    if i == 5:
        i = 1
    if i == 1:
        xy = xy + [-fib(j - 2), fib(j - 1)]
        c = c + [-fib(j - 2), 0]
    if i == 2:
        xy = xy + [-fib(j), -fib(j - 2)]
        c = c + [0, -fib(j - 2)]
    elif i == 3:
        xy = xy + [0, -fib(j)]
        c = c + [fib(j - 2), 0]
    elif i == 4:
        xy = xy + [fib(j - 1), 0]
        c = c + [0, fib(j - 2)]

    # Add a wedge
    rect = mpatches.Wedge([c[0], c[1]], fib(j), t[0], t[1], ec="none", lw=lw)
    patches.append(rect)

    # Add a rectangle
    rect = mpatches.Rectangle([xy[0], xy[1]], fib(j), fib(j), ec="none", lw=lw)
    # patches.append(rect)
    i += 1
    # label(xy + [0.5 * fib(j),  0.5 * fib(j)], "%s" % fib(j), j * 2)

colors = numpy.linspace(0, 0, 0)
collection = PatchCollection(patches, cmap=plt.cm.hsv, alpha=0.9)
collection.set_array(numpy.array(colors))
ax.add_collection(collection)
plt.axis('equal')
plt.axis('off')
plt.grid('off')

# plt.show()
plt.savefig('golden_spiral.png', dpi=600, bbox_inches='tight')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
