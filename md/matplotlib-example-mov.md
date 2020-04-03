---
title: matplotlib example mov (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'mov'


Modules used in program: 
* `import matplotlib as mpl`
* `import matplotlib.style`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import pandas as pd`

## python mov

Python matplotlib example: mov

```python
import pandas as pd
import numpy as np

import matplotlib.pyplot as plt
%matplotlib inline

import matplotlib.style
import matplotlib as mpl
mpl.style.use('ggplot')

from matplotlib.pylab import rcParams
rcParams['figure.figsize'] = 20, 10

from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler(feature_range=(0, 1))

# Creating copy of goog_data dataframe for moving averages

df = goog_data

df['Date'] = pd.to_datetime(df.Date, format='%Y-%m-%d')
df.index = df['Date']



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
