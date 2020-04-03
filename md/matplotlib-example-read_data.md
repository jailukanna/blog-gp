---
title: matplotlib example read data (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'read data'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import os`
* `import numpy as np`
* `import pandas as pd`

## python read data

Python matplotlib example: read data

```python
# import packages
import pandas as pd
import numpy as np
import os
from datetime import timedelta, datetime

import matplotlib
import matplotlib.pyplot as plt

%matplotlib inline
matplotlib.rcParams['figure.figsize'] = (12,8)

from sklearn.metrics import mean_squared_error
pd.options.mode.chained_assignment = None


# read the data
df = pd.read_csv('covid_19_clean_complete.csv')
df

# examining the data
df.info()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
