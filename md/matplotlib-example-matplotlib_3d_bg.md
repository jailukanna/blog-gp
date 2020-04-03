---
title: matplotlib example matplotlib 3d bg (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib 3d bg'


Modules used in program: 
* `import numpy as np`
* `import matplotlib`

## python matplotlib 3d bg

Python matplotlib example: matplotlib 3d bg

```python
from matplotlib.pyplot import *
import matplotlib
import numpy as np

cdict = {'red': ((0., 1, 1),
                 (0.05, 1, 1),
                 (0.11, 0, 0),
                 (0.66, 1, 1),
                 (0.89, 1, 1),
                 (1, 0.5, 0.5)),
         'green': ((0., 1, 1),
                   (0.05, 1, 1),
                   (0.11, 0, 0),
                   (0.375, 1, 1),
                   (0.64, 1, 1),
                   (0.91, 0, 0),
                   (1, 0, 0)),
         'blue': ((0., 1, 1),
                  (0.05, 1, 1),
                  (0.11, 1, 1),
                  (0.34, 1, 1),
                  (0.65, 0, 0),
                  (1, 0, 0))}

my_cmap = matplotlib.colors.LinearSegmentedColormap('my_colormap',cdict,256)

pcolor(np.random.rand(10,10),cmap=my_cmap)
colorbar()
show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
