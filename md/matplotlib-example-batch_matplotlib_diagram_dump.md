---
title: matplotlib example batch matplotlib diagram dump (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'batch matplotlib diagram dump'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import seaborn as sns`
* `import pandas as pd`
* `import matplotlib.pyplot as plt`
* `import seaborn as sns`
* `import pandas as pd`

## python batch matplotlib diagram dump

Python matplotlib example: batch matplotlib diagram dump

```python
from glob import glob
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

sns.set_style("darkgrid")
sns.set(rc = {"figure.figsize": (20, 10)})
sns.set(rc = {"figure.titlesize": "start-large"})

filename_wildcard = '*.gz'
df = pd.concat(
        [pd.read_csv(fn, parse_dates = True, index_col = "timestamp") for fn in glob(filename_wildcard)],
        sort = True,
)
#     df = df[tags]
df.sort_index(inplace = True)

from glob import glob
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

sns.set_style("darkgrid")
sns.set(rc = {"figure.figsize": (20, 10)})

filename_wildcard = '*.gz'
df = pd.concat(
        [pd.read_csv(fn, parse_dates = True, index_col = "timestamp") for fn in glob(filename_wildcard)],
        sort = True,
)
df.sort_index(inplace = True)

_30_mins = 1_800
min_value_count = 1000
tag = '01_dv_01_si_daca_pv'
threshold = 2
first, last = 0, -1
sub_direction = 'diagrams'
diagram_prefix = '01'

for idx, (start, end) in enumerate(zip(range(0, len(df), _30_mins), range(_30_mins, len(df), _30_mins))):
    plt.figure()
    ax = None
    data = df[tag].iloc[start:end]
    if len(data[data > threshold]) > min_value_count:
        start_time = str(df.iloc[start:end].index[first])
        end_time = str(df.iloc[start:end].index[last])

        ax = (df[tag].iloc[start:end].plot(title =
                                           f'Figure {idx} \n Start: {start_time} \n End: {end_time}'))

        ax.title.set_size(18)
        ax.set_xlabel("Time  (1 second Samples)")
        ax.set_ylabel("DV01 Speed RPM")
        start_time_str = start_time.replace('-', '_').replace(':', '_')
        end_time_str = end_time.replace('-', '_').replace(':', '_')
        plt.savefig(f'{sub_direction}/week_{diagram_prefix}_fig_{idx}_start_{start_time_str}_end_{end_time_str}.png')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
