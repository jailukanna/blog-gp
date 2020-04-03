---
title: matplotlib example matplotlib lato (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib lato'


Modules used in program: 
* `import matplotlib.pyplot as plt`

## python matplotlib lato

Python matplotlib example: matplotlib lato

```python
"""
Definitions to use Lato font with matplotlib
"""

import matplotlib.pyplot as plt

# Save matplotlibe INLINE's parameters
ipython.magic(u"%matplotlib inline")
inline_rc = dict(matplotlib.rcParams)

plt.rcdefaults()
plt.rcParams.update(inline_rc)
plt.rcParams['axes.facecolor'] = 'white'

plt.rcParams['font.family'] = 'sans-serif'
plt.rcParams['font.sans-serif'] = ['Lato']
plt.rcParams['font.weight'] = 100
plt.rcParams["text.latex.unicode"] = True
plt.rcParams["xtick.direction"] = 'out'
plt.rcParams["ytick.direction"] = 'out'

# Working:
matplotlib.rcParams['mathtext.fontset'] = 'custom'
matplotlib.rcParams['mathtext.it'] = 'sans:italic'
matplotlib.rcParams['mathtext.bf'] = 'sans:bold:italic'
matplotlib.rcParams['mathtext.sf'] = 'sans'
matplotlib.rcParams['mathtext.default'] = 'it'
matplotlib.rcParams['mathtext.rm'] = 'sans'

plt.rcParams['font.size'] = 24
plt.rcParams['axes.labelsize'] = 24
plt.rcParams['legend.fontsize'] = 16
plt.rcParams['axes.fontweight'] = 100

plt.rcParams.update()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
