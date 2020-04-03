---
title: matplotlib example mpl pickle pc test (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mpl pickle pc test'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import cPickle as pickle`
* `import matplotlib`

## python mpl pickle pc test

Python matplotlib example: mpl pickle pc test

```python
use_try = False # use try-except to ignore AttributeError

import matplotlib
matplotlib.rcdefaults()

import cPickle as pickle
import numpy as np
import matplotlib.pyplot as plt

from matplotlib.patches import Rectangle
from matplotlib.collections import PatchCollection

## set up plot of four rectangles around the origin
values = np.array([1, 2, 3, 4])
cmap = 'gist_rainbow'

x, y = ([-1, -1, 0, 0], [-1, 0, -1, 0])
dx, dy = (1, 1)

patches = [Rectangle(xy, width=dx, height=dy) for xy in zip(x, y)]
pc = PatchCollection(patches)

## dump pickle before setting any attributes on the PatchCollection
pickle.dump(pc, open('patch_collection_temp.pickle', 'w'))

# set attributes
pc.set_array(values)
pc.set_cmap(cmap)

# plot
fig, ax = plt.subplots()
ax.add_collection(pc)
ax.axis([-1, 1, -1, 1])
fig.savefig('pre_pickle_mpl_%s.png' % (matplotlib.__version__))
plt.clf()

## load the pickled PatchCollection
pc_loaded = pickle.load(open('patch_collection_temp.pickle'))

# set attributes
pc_loaded.set_array(values)
if use_try:
    try:
        pc_loaded.set_cmap(cmap)
    except AttributeError:
        pass
else:
    pc_loaded.set_cmap(cmap)

# plot
fig, ax = plt.subplots()
ax.add_collection(pc_loaded)
ax.axis([-1, 1, -1, 1])
fig.savefig('post_pickle_mpl_%s.png' % (matplotlib.__version__))
plt.clf()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
