---
title: matplotlib example test pyplot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'test pyplot'


Modules used in program: 
* `import guppy`
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib as mpl`

## python test pyplot

Python matplotlib example: test pyplot

```python

# Original
#
# Memory usage (iteration, object count, memory size)
# 100 431941 134768552
# 200 862736 269479552
# 300 1295682 405239168
# 400 1726603 539967136
# 500 2157948 674784872
# 600 2589067 809598080
# 700 3020848 944697920
# 800 3451516 1079398704
# 900 3884074 1214923736
# 
# 2542.85 real      2535.49 user         6.66 sys

import matplotlib as mpl
mpl.use('Agg')
import matplotlib.pyplot as plt

import numpy as np

import guppy

heapy = guppy.hpy()

mem = open('memory_pyplot.txt', 'wb')

heapy.setref()

for i in range(1000):

    if i % 100 == 0:
        h = heapy.heap()
        mem.write('%i %s %s\n' % (i, h.count, h.size))

    fig = plt.figure()
    ax = fig.add_subplot(1, 1, 1)
    ax.scatter(np.random.random(10), np.random.random(10))
    fig.savefig('test.png')

    mem.flush()

mem.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
