---
title: matplotlib example test pyplot close (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'test pyplot close'


Modules used in program: 
* `import guppy`
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib as mpl`

## python test pyplot close

Python matplotlib example: test pyplot close

```python
# Added plt.close()
#
# Memory usage (iteration, object count, memory size)
# 100 5461 1464216
# 200 5683 1569464
# 300 5610 1527120
# 400 5798 1617432
# 500 5320 1366000
# 600 5610 1530224
# 700 5724 1579600
# 800 5610 1527040
# 900 5616 1514112
#
# 271.73 real       270.46 user         0.77 sys


import matplotlib as mpl
mpl.use('Agg')
import matplotlib.pyplot as plt

import numpy as np

import guppy

heapy = guppy.hpy()

mem = open('memory_pyplot_close.txt', 'wb')

heapy.setref()

for i in range(1000):

    if i % 100 == 0:
        h = heapy.heap()
        mem.write('%i %s %s\n' % (i, h.count, h.size))

    fig = plt.figure()
    ax = fig.add_subplot(1, 1, 1)
    ax.scatter(np.random.random(10), np.random.random(10))
    fig.savefig('test.png')

    plt.close()

mem.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
