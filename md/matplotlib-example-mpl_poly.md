---
title: matplotlib example mpl poly (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mpl poly'


Modules used in program: 
* `import matplotlib`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python mpl poly

Python matplotlib example: mpl poly

```python
import numpy as np

import matplotlib.pyplot as plt
import matplotlib
from matplotlib.patches import Polygon
from matplotlib.collections import PatchCollection

fig, ax = plt.subplots()
patches = []
num_polygons = 5
num_sides = 5

for i in range(num_polygons):
    polygon = Polygon(np.random.rand(num_sides ,2), True)
    patches.append(polygon)

p = PatchCollection(patches, cmap=matplotlib.cm.jet, alpha=0.4)

colors = 100*np.random.rand(len(patches))
p.set_array(np.array(colors))

ax.add_collection(p)

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
