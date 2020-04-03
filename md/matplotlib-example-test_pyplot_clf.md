---
title: matplotlib example test pyplot clf (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'test pyplot clf'


Modules used in program: 
* `import guppy`
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib as mpl`

## python test pyplot clf

Python matplotlib example: test pyplot clf

```python
# Added fig.clf()
#
# Memory usage (iteration, object count, memory size)
# 100 24523  7963408
# 200 43692  14487880
# 300 63681  21294464
# 400 82909  27825696
# 500 102409 34451408
# 600 121637 40974992
# 700 141545 47740552
# 800 160909 54312208
# 900 180409 60949200
#
# 454.47 real       450.69 user         3.20 sys


import matplotlib as mpl
mpl.use('Agg')
import matplotlib.pyplot as plt

import numpy as np

import guppy

heapy = guppy.hpy()

mem = open('memory_pyplot_clf.txt', 'wb')

heapy.setref()

for i in range(1000):

    if i % 100 == 0:
        h = heapy.heap()
        mem.write('%i %s %s\n' % (i, h.count, h.size))

    fig = plt.figure()
    ax = fig.add_subplot(1, 1, 1)
    ax.scatter(np.random.random(10), np.random.random(10))
    fig.savefig('test.png')

    fig.clf()

mem.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
