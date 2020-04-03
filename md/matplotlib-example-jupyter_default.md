---
title: matplotlib example jupyter default (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'jupyter default'

Functions in program: 
* `def savefig(name, path='../../results/figures'):`

Modules used in program: 
* `import seaborn as sns`
* `import matplotlib.colors`
* `import matplotlib.pyplot as plt`
* `import json`
* `import glob`
* `import time`
* `import datetime`
* `import re`
* `import sys`
* `import os`
* `import numpy as np`
* `import pandas as pd`

## python jupyter default

Python matplotlib example: jupyter default

```python
import pandas as pd
import numpy as np
import os
import sys
import re
import datetime
import time
import glob
import json
from tqdm import tqdm, tqdm_notebook
from colorama import Fore, Style

from dotenv import load_dotenv
load_dotenv('../../.env')

%matplotlib inline
import matplotlib.pyplot as plt
import matplotlib.colors
import seaborn as sns

%config InlineBackend.figure_format='retina'
sns.set() # Revert to matplotlib defaults
plt.style.use('ggplot')
# plt.style.use('fivethirtyeight')

plt.rcParams['figure.figsize'] = (12, 8)
plt.rcParams['legend.fancybox'] = True
plt.rcParams['axes.labelpad'] = 20
plt.rcParams['grid.alpha'] = 0.2
plt.rcParams['axes.facecolor'] = 'white'
plt.rcParams['figure.facecolor'] = 'white'
plt.rcParams['savefig.facecolor'] = 'white'
plt.rcParams['xtick.major.pad'] = 15
plt.rcParams['xtick.minor.pad'] = 15
plt.rcParams['ytick.major.pad'] = 10
plt.rcParams['ytick.minor.pad'] = 10

SMALL_SIZE, MEDIUM_SIZE, BIGGER_SIZE = 14, 16, 20
plt.rc('font', size=SMALL_SIZE)
plt.rc('axes', titlesize=SMALL_SIZE)
plt.rc('axes', labelsize=MEDIUM_SIZE)
plt.rc('xtick', labelsize=SMALL_SIZE)
plt.rc('ytick', labelsize=SMALL_SIZE)
plt.rc('legend', fontsize=MEDIUM_SIZE)
plt.rc('axes', titlesize=BIGGER_SIZE)


def savefig(name, path='../../results/figures'):
    f_path = os.path.join(path, f'{name}.png')
    print('Saving figure to file: {}'.format(f_path))
    plt.savefig(f_path, bbox_inches='tight', dpi=300)

%reload_ext autoreload
%autoreload 2
    
%reload_ext version_information
%version_information pandas, numpy

from IPython.display import HTML
HTML('<style>div.text_cell_render{font-size:130%;padding-top:50px;padding-bottom:50px}</style>')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
