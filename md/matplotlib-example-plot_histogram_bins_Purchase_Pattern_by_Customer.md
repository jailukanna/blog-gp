---
title: matplotlib example plot histogram bins Purchase Pattern by Customer (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot histogram bins Purchase Pattern by Customer'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import pandas as pd`

## python plot histogram bins Purchase Pattern by Customer

Python matplotlib example: plot histogram bins Purchase Pattern by Customer

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

purchase_plot = purchase_patterns['ext price'].hist(bins=20)
purchase_plot.set_title("Purchase Patterns")
purchase_plot.set_xlabel("Order Amount($)")
purchase_plot.set_ylabel("Number of orders")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
