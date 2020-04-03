---
title: matplotlib example tiny smooth jet (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'tiny smooth jet'


Modules used in program: 
* `import scipy.interpolate`
* `import matplotlib.cm`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python tiny smooth jet

Python matplotlib example: tiny smooth jet

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.colors import LinearSegmentedColormap

from parula import test_cm as parula
from option_d import test_cm as viridis

import matplotlib.cm
import scipy.interpolate

################################################################

from colorspacious import cspace_convert

# Get RGB values for jet
jet_rgb = matplotlib.cm.jet(np.linspace(0, 1, 1000))[:, :3]
#jet_rgb = parula(np.linspace(0, 1, 1000))[:, :3]
#jet_rgb = viridis(np.linspace(0, 1, 1000))[:, :3]

# Switch into perceptual space
jet_u = cspace_convert(jet_rgb, "sRGB1", "CAM02-UCS")

# Calculate distance between sample i and sample i+1
dists = np.sqrt(np.sum((jet_u[:-1, :] - jet_u[1:, :]) ** 2, axis=-1))

# Calculate total distance from sample 0 to sample i
arclengths = np.concatenate(([0], np.cumsum(dists)))

# Interpolate new points that are evenly spaced along this arc
interpolator = scipy.interpolate.interp1d(arclengths, jet_u, axis=0)
newjet_u = interpolator(np.linspace(1e-10, np.max(arclengths), 1000))

# Back to RGB space
newjet_rgb = cspace_convert(newjet_u, "CAM02-UCS", "sRGB1")

################################################################

newjet = LinearSegmentedColormap.from_list("newjet",
                                           np.clip(newjet_rgb, 0, 1))
# from pycam02ucs.cm.viscm import viscm
from viscm import viscm
viscm(newjet, "CAM02-UCS")
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
