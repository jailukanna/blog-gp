---
title: matplotlib example jupyter comparisons (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'jupyter comparisons'


Modules used in program: 
* `import matplotlib  as plt`
* `import pandas as pd`
* `import matplotlib  as plt`
* `import pandas as pd`
* `import matplotlib  as plt`
* `import pandas as pd`

## python jupyter comparisons

Python matplotlib example: jupyter comparisons

```python
'''
   This code uses bar plots to compare the percentages of women (ShareWomen) from the 10 first and last rows of a sorted dataframe.
   
'''


import pandas as pd
import matplotlib  as plt
%matplotlib inline

recent_grads = pd.read_csv("recent-grads.csv")
f_row = recent_grads.iloc[1]
f_row.head()
f_row.tail()
recent_grads.describe()

raw_data_count = len(recent_grads.axes[0])
recent_grads = recent_grads.dropna()

cleaned_data_count = len(recent_grads.axes[0])

print(raw_data_count)
print(cleaned_data_count)

recent_grads.plot(x='Sample_size', y='Median', kind='scatter')
recent_grads.plot(x='Sample_size', y='Unemployment_rate', kind='scatter')
recent_grads.plot(x='Full_time',   y='Median', kind='scatter')
recent_grads.plot(x='ShareWomen',  y='Unemployment_rate', kind='scatter')
recent_grads.plot(x='Men', y='Median', kind='scatter')
recent_grads.plot(x='Women', y='Median', kind='scatter')


## Continued...

import pandas as pd
import matplotlib  as plt
%matplotlib inline

recent_grads = pd.read_csv("recent-grads.csv")
f_row = recent_grads.iloc[1]
f_row.head()
f_row.tail()
recent_grads.describe()

raw_data_count = len(recent_grads.axes[0])
recent_grads = recent_grads.dropna()

cleaned_data_count = len(recent_grads.axes[0])

print(raw_data_count)
print(cleaned_data_count)

ax1 = recent_grads.plot(x='Sample_size', y='Rank', kind='scatter')
ax1 = ax1.set_yticklabels(recent_grads['Major'])

ax = recent_grads.plot(x='Major_code', y='ShareWomen', kind='scatter')
#ax.set_xticklabels(recent_grads['Major'], rotation=90)
#recent_grads.plot(x='Major', y='Sample_size', kind='bar')
recent_grads.plot(x='Full_time',   y='Median', kind='scatter')

# Continued

import pandas as pd
import matplotlib  as plt
%matplotlib inline

recent_grads = pd.read_csv("recent-grads.csv")
f_row = recent_grads.iloc[1]
f_row.head()
f_row.tail()
recent_grads.describe()

raw_data_count = len(recent_grads.axes[0])
recent_grads = recent_grads.dropna()

cleaned_data_count = len(recent_grads.axes[0])









```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
