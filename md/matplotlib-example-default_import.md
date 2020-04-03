---
title: matplotlib example default import (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'default import'

Functions in program: 
* `def generate_cmap(colors):`
* `def to_pickle(filename, obj):`
* `def unpickle(filename):`

Modules used in program: 
* `import rpy2.robjects as robjects`
* `import rpy2`
* `import  pickle`
* `import seaborn as sns`
* `import matplotlib.cm as cm`
* `import matplotlib.pyplot as plt`
* `import multiprocessing as mp`
* `import scipy as sp`
* `import pandas as pd`
* `import numpy.random as rd`
* `import numpy as np`
* `import math, sys, functools, os`

## python default import

Python matplotlib example: default import

```python
import math, sys, functools, os
import numpy as np
import numpy.random as rd
from numpy import matrix
import pandas as pd
import scipy as sp
from scipy import stats as st
from datetime import  datetime as dt
from collections import Counter
from itertools import chain
import multiprocessing as mp

import matplotlib.pyplot as plt
import matplotlib.cm as cm
import seaborn as sns
sns.set(style="whitegrid", palette="muted", color_codes=True)
%matplotlib inline

import  pickle
def unpickle(filename):
    with open(filename, 'rb') as fo:
        p = pickle.load(fo)
    return p

def to_pickle(filename, obj):
    with open(filename, 'wb') as f:
        pickle.dump(obj, f, -1)

from matplotlib.colors import LinearSegmentedColormap

def generate_cmap(colors):
    """自分で定義したカラーマップを返す"""
    values = range(len(colors))

    vmax = np.ceil(np.max(values))
    color_list = []
    for v, c in zip(values, colors):
        color_list.append( ( v/ vmax, c) )
    return LinearSegmentedColormap.from_list('custom_cmap', color_list)

cm = generate_cmap(['lightblue', 'mediumblue', 'mediumblue','black', 'red', 'red', 'orangered'])

from IPython.html.widgets import interact, interactive, fixed
from IPython.html import widgets

import rpy2
from rpy2.robjects.packages import importr
import rpy2.robjects as robjects
from rpy2.robjects.functions import SignatureTranslatedFunction

# import pandas.rpy.common as com   # [depricated]
from rpy2.robjects import pandas2ri
pandas2ri.activate()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
