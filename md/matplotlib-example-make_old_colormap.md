---
title: matplotlib example make old colormap (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'make old colormap'


## python make old colormap

Python matplotlib example: make old colormap

```python
"""
Richard Campen, 2017

Before matplotlib version 2.0 the set of qualitative colormaps 'Accent', 'Dark2', 'Paired', 'Pastel1', 'Pastel2', 'Set1', 
'Set2', and 'Set3' were continuous colormaps. As of matplotlib 2.0 these colormaps are discrete as per the change logs 
(http://matplotlib.org/users/whats_new.html#qualitative-colormaps).

To recreate the continuous form of these colormaps as per pre 2.0 use the below code, changing the name of the colormap 
as necessary.

"""

from pylab import imshow, rand, colorbar
from matplotlib.cm import get_cmap
from matplotlib.colors import LinearSegmentedColormap

current_cmap = get_cmap('Set1')
cmap_colors = [[rgb for rgb in color] for color in current_cmap.colors]  
new_cmap = LinearSegmentedColormap.from_list('new_cmap', cmap_colors)

# # Run the below to visualise the new colorbar in a jupyter notebook
# %matplotlib inline
# imshow(rand(18,20), cmap=new_cmap, vmin=0, vmax=1, interpolation="nearest") 
# colorbar()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
