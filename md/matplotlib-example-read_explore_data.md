---
title: matplotlib example read explore data (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'read explore data'


Modules used in program: 
* `import matplotlib`
* `import matplotlib.mlab as mlab`
* `import matplotlib.pyplot as plt`
* `import seaborn as sns`
* `import numpy as np`
* `import pandas as pd`

## python read explore data

Python matplotlib example: read explore data

```python
# import packages
import pandas as pd
import numpy as np
import seaborn as sns

import matplotlib.pyplot as plt
import matplotlib.mlab as mlab
import matplotlib
plt.style.use('ggplot')
from matplotlib.pyplot import figure

%matplotlib inline
matplotlib.rcParams['figure.figsize'] = (12,8)

pd.options.mode.chained_assignment = None



# read the data
df = pd.read_csv('sberbank.csv')

# shape and data types of the data
print(df.shape)
print(df.dtypes)

# select numeric columns
df_numeric = df.select_dtypes(include=[np.number])
numeric_cols = df_numeric.columns.values
print(numeric_cols)

# select non numeric columns
df_non_numeric = df.select_dtypes(exclude=[np.number])
non_numeric_cols = df_non_numeric.columns.values
print(non_numeric_cols)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
