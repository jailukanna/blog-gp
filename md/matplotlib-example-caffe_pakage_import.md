---
title: matplotlib example caffe pakage import (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'caffe pakage import'


Modules used in program: 
* `import os`
* `import caffe`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python caffe pakage import

Python matplotlib example: caffe pakage import

```python
# set up Python environment: numpy for numerical routines, and matplotlib for plotting
import numpy as np
import matplotlib.pyplot as plt
# display plots in this notebook
%matplotlib inline

# set display defaults
plt.rcParams['figure.figsize'] = (10, 10)        # large images
plt.rcParams['image.interpolation'] = 'nearest'  # don't interpolate: show square pixels
plt.rcParams['image.cmap'] = 'gray'  # use grayscale output rather than a (potentially misleading) color heatmap

import caffe
# If you get "No module named _caffe", either you have not built pycaffe or you have the wrong path.

import os


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
