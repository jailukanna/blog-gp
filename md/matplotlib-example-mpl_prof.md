---
title: matplotlib example mpl prof (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mpl prof'

Functions in program: 
* `def line_collection(t, spikes):`

Modules used in program: 
* `import time`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python mpl prof

Python matplotlib example: mpl prof

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.collections import LineCollection

def line_collection(t, spikes):

    ax = plt.subplot(111)
    ts, ns = spikes.shape
    segs = np.zeros((ns, ts, 2), np.float32)
    segs[:, :, 1] = spikes.T
    segs[:, :, 0] = t[:, None].T

    line_segments = LineCollection(segs)

    ax.add_collection(line_segments)
    
    ax.set_xlim(t.min(), t.max())
    ax.set_ylim(spikes.min(), spikes.max())
    



t = np.linspace(0, np.pi*2, 34)
y = np.sin(t) 
spikes = y[:, None]*np.random.rand(1,20000)

import time

start1 = time.time()
#plt.plot(t, spikes, '-')
line_collection(t, spikes)
start2 = time.time()
plt.savefig('plot_perf.png')
stop = time.time()
print('Plotting:', -start1+start2 )
print('Exporting:', -start2+stop)
print('total:', -start1+stop)

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
