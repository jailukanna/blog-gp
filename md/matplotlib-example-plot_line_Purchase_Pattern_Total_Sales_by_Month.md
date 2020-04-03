---
title: matplotlib example plot line Purchase Pattern Total Sales by Month (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot line Purchase Pattern Total Sales by Month'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import pandas as pd`

## python plot line Purchase Pattern Total Sales by Month

Python matplotlib example: plot line Purchase Pattern Total Sales by Month

```python
"""
B-006 - Simple Graphing with IPython and Pandas
https://pbpython.com/simple-graphing-pandas.html
"""

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

sales=pd.read_csv("https://pbpython.com/extras/sample-salesv2.csv", parse_dates=['date'])

purchase_patterns = sales[['ext price','date']]
purchase_patterns = purchase_patterns.set_index('date')

purchase_plot = purchase_patterns.resample('M').sum().plot(title="Total Sales by Month",legend=None)

fig = purchase_plot.get_figure()

# fig.savefig("total-sales.png")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
