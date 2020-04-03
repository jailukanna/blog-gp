---
title: matplotlib example startup (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'startup'


Modules used in program: 
* `import numpy as np`
* `import pandas as pd`

## python startup

Python matplotlib example: startup

```python
%matplotlib inline
%config InlineBackend.figure_format='retina'
from __future__ import absolute_import, division, print_function
import pandas as pd
from matplotlib import pyplot as plt
import numpy as np


plt.rcParams["font.family"] = "arial"
plt.rcParams["axes.linewidth"] = "1.0"
plt.rcParams["xtick.major.width"] = "1.0"
plt.rcParams["ytick.major.width"] = "1.0"
plt.rcParams["xtick.minor.width"] = "1.0"
plt.rcParams["ytick.minor.width"] = "1.0"
plt.rcParams["xtick.labelsize"] = "12.0"
plt.rcParams["ytick.labelsize"] = "12.0"
plt.rcParams["font.weight"] = "500"

pd.options.display.max_columns = 100
pd.options.display.max_rows = 100

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
