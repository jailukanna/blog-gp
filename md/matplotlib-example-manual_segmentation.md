---
title: matplotlib example manual segmentation (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'manual segmentation'

Functions in program: 
* `def select_line(image):`

Modules used in program: 
* `import matplotlib.pyplot as plt`

## python manual segmentation

Python matplotlib example: manual segmentation

```python
import matplotlib.pyplot as plt


def select_line(image):
    """Interactively select a line with matplotlib.
    
    Adapted from this StackOverflow answer by user dawe:
    https://stackoverflow.com/questions/9136938/matplotlib-interactive-graphing-manually-drawing-lines-on-a-graph
    
    Parameters
    ----------
    image : AdornedImage

    Returns
    -------
    line_coordinates
        Row, column format & units of pixels.
    """
    image = fibsem.conversions._convert_to_numpy(image)
    fig, ax = plt.subplots(1)
    ax.imshow(image, cmap='gray')
    ax = plt.gca()
    xy = plt.ginput(2)
    x = [p[0] for p in xy]
    y = [p[1] for p in xy]
    line = plt.plot(x, y, 'y')
    ax.figure.canvas.draw()
    line_coordinates = [(xy[0][1], xy[0][0]), (xy[1][1], xy[1][0])]
    return line_coordinates


# Note that this deprecation warning is produced:
# site-packages\matplotlib\backend_bases.py:2445: MatplotlibDeprecationWarning: Using default event loop until function specific to this GUI is implemented
#   warnings.warn(str, mplDeprecation)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
