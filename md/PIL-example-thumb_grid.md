---
title: PIL example thumb grid (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'thumb grid'

Functions in program: 
* `def thumb_grid(im_list, grid_shape, scale=0.1, axes_pad=0.07):`

Modules used in program: 
* `import PIL`
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python thumb grid

Python PIL example: thumb grid

```python
#!/usr/bin/env python

"""
Display a grid of thumbnails.
"""

import matplotlib.pyplot as plt
from mpl_toolkits.axes_grid1 import ImageGrid
import numpy as np
import PIL

def thumb_grid(im_list, grid_shape, scale=0.1, axes_pad=0.07):
    """
    Display the specified PIL images in a grid.

    Parameters
    ----------
    im_list : list of numpy.ndarray instances
        Bitmaps to display.
    grid_shape : tuple
        Grid shape.
    scale : float
        Scaling factor; 1 is 100%.
    axes_pad : float or (float, float)
        Padding between axes, in inches.
    """

    # Grid must be 2D:
    assert len(grid_shape) == 2

    # Make sure all images can fit in grid:
    assert np.prod(grid_shape) >= len(im_list)

    grid = ImageGrid(plt.gcf(), 111, grid_shape, axes_pad=axes_pad)
    for i, data in enumerate(im_list):

        # Scale image:
        im = PIL.Image.fromarray(data)
        thumb_shape = [int(scale*j) for j in im.size]
        im.thumbnail(thumb_shape, PIL.Image.ANTIALIAS)
        data_thumb = np.array(im)
        grid[i].imshow(data_thumb)

        # Turn off axes:
        grid[i].axes.get_xaxis().set_visible(False)
        grid[i].axes.get_yaxis().set_visible(False)

if __name__ == '__main__':
    import requests

    r = requests.get('https://upload.wikimedia.org/wikipedia/commons/7/79/Hilbert.jpg',
                     stream=True)
    im = np.array(PIL.Image.open(r.raw))

    N = 16
    im_list = [im for i in range(N)]
    thumb_grid(im_list, (4, 4))
    plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
