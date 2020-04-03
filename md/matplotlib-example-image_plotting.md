---
title: matplotlib example image plotting (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'image plotting'

Functions in program: 
* `def plot_images(images, axes=None, width=22, cmap=None, rows=None, cols=None):`
* `def plot_image(image, ax=None, cmap=None):`

Modules used in program: 
* `import matplotlib.pyplot as plt`

## python image plotting

Python matplotlib example: image plotting

```python
import matplotlib.pyplot as plt

def plot_image(image, ax=None, cmap=None):
    if ax is None:
        _, ax = plt.subplots()
    ax.imshow(image, cmap=cmap)
    ax.set_xticklabels([])
    ax.set_yticklabels([])
    ax.set_aspect('equal')
    ax.set_axis_off()
    return ax

def plot_images(images, axes=None, width=22, cmap=None, rows=None, cols=None):
    """ Function to plot multiple images.
    
    Args:
        images: array of images to be plotted
        axes: optional axes array to be used. If this is passed, rows and cols
              are ignored
        width: the width of the plot to be passed to plt.figure(...)
        cmap: the cmap to be passed to plt.imshow()
        rows: the number of rows of images. If not passed, will try to
              automatically determine, but may leave some images out
        cols: the number of columns of images. If not passed, will default
              to 8 unless rows is passed, in which case it will automatically
              determine the best number (but may leave some images out)
    """
    if (cols is None) is not (rows is None):
        if rows is None:
            rows = len(images) // cols
        if cols is None:
            cols = len(images) // rows
    if rows is None and cols is None:
        cols = min(len(images), 8)
        rows = len(images) // cols
    if axes is None:
        _, axes = plt.subplots(nrows=rows, ncols=cols, figsize=(width,width*rows/cols))
    for sample, ax in zip(images, axes.ravel()):
        plot_image(sample, ax=ax, cmap=cmap)
    plt.subplots_adjust(wspace=.05, hspace=.05)
    return axes

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
