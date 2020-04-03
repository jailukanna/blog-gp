---
title: matplotlib example ds boilerplate (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'ds boilerplate'


Modules used in program: 
* `import pandas as pd`
* `import warnings; warnings.filterwarnings("ignore")  # font warning`
* `import seaborn as sns`
* `import matplotlib`
* `import matplotlib.pyplot as plt`

## python ds boilerplate

Python matplotlib example: ds boilerplate

```python
import matplotlib.pyplot as plt
import matplotlib
import seaborn as sns
%matplotlib inline
plt.rcParams["figure.figsize"] = (12., 7.)
%config InlineBackend.figure_format = 'retina'

sns.set_style('dark')
matplotlib.rc('font', family='Arial')  # in case of missing cyrillic fonts
import warnings; warnings.filterwarnings("ignore")  # font warning

import pandas as pd
pd.set_option('display.width', 1000)
pd.set_option('display.max_columns', 500)

# how to control size
plt.gcf().set_figsize_inches((6, 3))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
