---
title: matplotlib example manual settings (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'manual settings'


Modules used in program: 
* `import matplotlib`
* `import seaborn as sns`
* `import matplotlib.pylab as plt`

## python manual settings

Python matplotlib example: manual settings

```python
import matplotlib.pylab as plt
import seaborn as sns
import matplotlib

sns.set_style('whitegrid')

matplotlib.rcParams['figure.figsize'] = [9, 5]
matplotlib.rcParams['figure.facecolor'] = "#f2ece3"
matplotlib.rcParams['axes.facecolor'] = "#f2ece3"
matplotlib.rcParams['legend.facecolor'] = "#f2ece3"
matplotlib.rcParams['text.color'] = '#595959'
matplotlib.rcParams['grid.color'] = '#d9d9d9'
matplotlib.rcParams['axes.edgecolor'] = '#d9d9d9'
matplotlib.rcParams['figure.titlesize'] = 18
matplotlib.rcParams['xtick.labelsize'] = 10
matplotlib.rcParams['ytick.labelsize'] = 14
matplotlib.rcParams['axes.labelsize'] = 14
matplotlib.rcParams['axes.titlesize'] = 16

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
