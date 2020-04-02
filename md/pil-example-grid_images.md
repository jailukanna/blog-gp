---
title: pil example grid images (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'grid images'

Functions in program: 
* `def thumb_grid(im_list, grid_shape, fname_out, scale=1.0, axes_pad=0.07, figsize=(7, 7), dpi=900):`

Modules used in program: 
* `import PIL`
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python grid images

Python pil example: grid images

```python
import matplotlib.pyplot as plt
from mpl_toolkits.axes_grid1 import ImageGrid
import numpy as np
import PIL

def thumb_grid(im_list, grid_shape, fname_out, scale=1.0, axes_pad=0.07, figsize=(7, 7), dpi=900):
    assert len(grid_shape) == 2 # Grid must be 2D:
    assert np.prod(grid_shape) >= len(im_list) # Make sure all images can fit in grid:
    plt.figure(figsize=figsize, dpi=dpi)

    grid = ImageGrid(plt.gcf(), 111, grid_shape, axes_pad=axes_pad)
    shape_index = np.argmin([np.prod(np.array(im.shape)) for im in im_list])
    thumb_shape = np.array(np.array(im_list[shape_index]).shape) * scale
    for i, im in enumerate(im_list):
        data_orig = im.copy()

        # Scale image:
        im = PIL.Image.fromarray(data_orig)
        im.thumbnail(thumb_shape, PIL.Image.ANTIALIAS)
        data_thumb = np.array(im)
        grid[i].imshow(data_thumb)

        # Turn off axes:
        grid[i].axes.get_xaxis().set_visible(False)
        grid[i].axes.get_yaxis().set_visible(False)

    plt.gca().set_axis_off()
    plt.subplots_adjust(top = 1, bottom = 0, right = 1, left = 0, hspace = 0, wspace = 0)
    plt.margins(0,0)
    plt.gca().xaxis.set_major_locator(plt.NullLocator())
    plt.gca().yaxis.set_major_locator(plt.NullLocator())
    plt.savefig(fname_out, bbox_inches = 'tight', pad_inches = 0.06)
    
# Usage: thumb_grid(images, (rows, columns), f'filename_output.jpeg')
# type(3D ndarray)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
