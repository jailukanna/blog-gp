---
title: matplotlib example setup matplotlib notebook (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'setup matplotlib notebook'

Functions in program: 
* `def suppress_warning():`
* `def set_style():`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib as mpl`
* `import warnings`

## python setup matplotlib notebook

Python matplotlib example: setup matplotlib notebook

```python
import warnings

import matplotlib as mpl
import matplotlib.pyplot as plt

def set_style():
    plt.style.use([
        'seaborn-notebook', 'seaborn-colorblind', {
            'figure.dpi': 96,
            'axes.grid': True,
            'grid.linestyle': ':',
            'legend.frameon': True,
            'lines.linewidth': 1
        }
    ])
    color_list = 'bgrmyc'

    cc = mpl.colors.colorConverter
    for code, prop in zip(color_list, plt.rcParams['axes.prop_cycle']):
        cc.colors[code] = cc.to_rgb(prop['color'])
    cc.cache = {}

def suppress_warning():
    warnings.filterwarnings('ignore',
                            category=UserWarning,
                            module='matplotlib.figure',
                            message='This figure includes Axes that are not compatible with tight_layout')

set_style()
suppress_warning()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
