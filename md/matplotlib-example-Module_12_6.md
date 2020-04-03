---
title: matplotlib example Module 12 6 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Module 12 6'


Modules used in program: 
* `import hvplot.pandas`
* `import hvplot`
* `import holoviews as hv`
* `import datetime`
* `import os`
* `import scipy as sp`
* `import numpy as np`
* `import seaborn as sns`
* `import matplotlib.pyplot as plt`
* `import pandas as pd`

## python Module 12 6

Python matplotlib example: Module 12 6

```python
#Import libraries necessary for a suite of data analyses.
import pandas as pd
from pandas import ExcelWriter
from pandas import ExcelFile
import matplotlib.pyplot as plt
from matplotlib.ticker import FuncFormatter
from matplotlib import rcParams
%matplotlib inline
import seaborn as sns
plt.style.use('seaborn-whitegrid')
import numpy as np
import scipy as sp
from scipy import stats
from scipy.stats import ttest_ind
import os
import datetime
from datetime import datetime
from pandas.plotting import register_matplotlib_converters
import holoviews as hv
from holoviews import dim, opts
hv.extension('bokeh')
import hvplot
import hvplot.pandas

#Load data
open_access_pubs = pd.read_csv('/home/vanellope/Documents/Thinkful/Module_12/WELLCOME_APCspend2013_forThinkful.csv', 
                               encoding = 'iso 8859-2')

#Look at the data.
display(open_access_pubs.head())
open_access_pubs.info()

#Rename columns to be manageable.
open_access_pubs.columns = ['id', 'publisher', 'journal', 'article', 'cost']
display(open_access_pubs.head())

#Split cost column into two new columns.
cost_split = open_access_pubs.cost.str.split('Ł', expand=True)
cost_split.columns = ['cost_unit', 'price']
pubs_parsed = pd.concat([open_access_pubs, cost_split], axis=1)
display(pubs_parsed.head())

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
