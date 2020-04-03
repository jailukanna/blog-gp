---
title: matplotlib example plot multiple line graphs (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot multiple line graphs'

Functions in program: 
* `def plot_multiple_likelihood_values(likelihood_arr, x=None, time_axis=0,`

Modules used in program: 
* `import importlib`
* `import h5py`
* `import pdb`
* `import seaborn as sns`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import os`
* `import sys`
* `import json`
* `import pandas as pd`
* `import numpy as np`

## python plot multiple line graphs

Python matplotlib example: plot multiple line graphs

```python
import numpy as np
import pandas as pd
import json
import sys
import os
import matplotlib
#matplotlib.use('Agg') 
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
from matplotlib import cm
from matplotlib.ticker import LinearLocator, FormatStrFormatter
import seaborn as sns
import pdb
import h5py
import importlib

# Add root folder to sys path
sys.path.append("../")
sys.path.append("../gridworld/matthew_ja_irl/")

# %pylab inline
# inline doesn't give interactive plots
%matplotlib inline 
# %matplotlib notebook
plt.rcParams['figure.figsize'] = (6.0, 5.0) # set default size of plots
plt.rcParams['image.interpolation'] = 'nearest'
plt.rcParams['image.cmap'] = 'Blues'

#sns.set_style("white")

# for auto-reloading external modules
# see http://stackoverflow.com/questions/1907993/autoreload-of-modules-in-ipython
%load_ext autoreload
%autoreload 2

sns.set()



def plot_multiple_likelihood_values(likelihood_arr, x=None, time_axis=0,
                                    save_path='', title='', xlabel='',
                                    ylabel='', colors=['red', 'green', 'blue'],
                                    labels=['red', 'green', 'blue'],
                                    use_max_min_range=True,
                                    legend_fontsize=18, xlabel_fontsize=20,
                                    ylabel_fontsize=20,
                                    figsize=(6,5)):
    
    assert len(likelihood_arr) <= len(colors), "Missing colors for plot."
    mean_likelihood, std_likelihood = [], []
    min_likelihood, max_likelihood = [], []
    if time_axis >= 0:
        for l in likelihood_arr:
            mean_likelihood.append(np.mean(l, axis=time_axis))
            std_likelihood.append(np.std(l, axis=time_axis))
            min_likelihood.append(np.min(l, axis=time_axis))
            max_likelihood.append(np.max(l, axis=time_axis))
    else:
        assert len(likelihood_arr[0].shape) == 1, \
            "Can only plot vector with -ve time axis"
        mean_likelihood = likelihood_arr
        std_likelihood = [np.zeros(likelihood_arr[0].shape)] * len(likelihood_arr)
    
    if x is None:
        x = range(len(mean_likelihood[0]))
    
    fig = plt.figure(figsize=figsize)
    ax = fig.add_subplot(111)
    plots = []
    for i in xrange(len(mean_likelihood)):
        m, s = mean_likelihood[i], std_likelihood[i]
        min_val, max_val = min_likelihood[i], max_likelihood[i]
        c = colors[i]
        p, = ax.plot(x, m, 'k', color=c, label=labels[i])
        if use_max_min_range:
            ax.fill_between(x, min_val, max_val,
                             alpha=0.2, edgecolor=c, facecolor=c,                         
                             linewidth=4, linestyle='dashdot', antialiased=True)
        else:
            ax.fill_between(x, m - s, m + s,
                             alpha=0.2, edgecolor=c, facecolor=c,                         
                             linewidth=4, linestyle='dashdot', antialiased=True)
        plots.append(p)
        
    ax.legend(handles=plots, fontsize=legend_fontsize)
    ax.set_title(title)
    ax.set_xlabel(xlabel, fontsize=xlabel_fontsize)
    ax.set_ylabel(ylabel, fontsize=ylabel_fontsize)

    if len(save_path) > 0:
        plt.savefig(save_path, bbox_inches='tight')
    plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
