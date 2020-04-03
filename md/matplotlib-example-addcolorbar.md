---
title: matplotlib example addcolorbar (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'addcolorbar'

Functions in program: 
* `def addcolorbar(ax, im, pos='right', size='5%', pad=0.05, orientation='vertical',`

## python addcolorbar

Python matplotlib example: addcolorbar

```python
def addcolorbar(ax, im, pos='right', size='5%', pad=0.05, orientation='vertical',
                stub=False, max_ticks=None, label=None):
    '''
    add a colorbar to a matplotlib image.
    
    ax -- the axis object the image is drawn in
    im -- the image (return value of ax.imshow(...))
    
    When changed, please update:
    https://gist.github.com/skuschel/85f0645bd6e37509164510290435a85a
    
    Stephan Kuschel, 2018
    '''
    from mpl_toolkits.axes_grid1 import make_axes_locatable
    import matplotlib.pyplot as plt
    divider = make_axes_locatable(ax)
    cax = divider.append_axes(pos, size=size, pad=pad)
    if stub:
        cax.set_visible(False)
        return cax
    
    cb = plt.colorbar(im, cax=cax, orientation=orientation)
    if max_ticks is not None:
        from matplotlib import ticker
        tick_locator = ticker.MaxNLocator(nbins=max_ticks)
        cb.locator = tick_locator
        cb.update_ticks()
    if label is not None:
        cb.set_label(label)
    return cax

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
