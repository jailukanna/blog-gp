---
title: matplotlib example plot bar Total Sales by Customer (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot bar Total Sales by Customer'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import pandas as pd`

## python plot bar Total Sales by Customer

Python matplotlib example: plot bar Total Sales by Customer

```python
"""
B-006 - Simple Graphing with IPython and Pandas
https://pbpython.com/simple-graphing-pandas.html
"""

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

sales=pd.read_csv("https://pbpython.com/extras/sample-salesv2.csv", parse_dates=['date'])
customers = sales[['name','ext price','date']]
customer_group = customers.groupby('name')
sales_totals = customer_group.sum()

my_plot = sales_totals.plot(kind='bar')

•sorting the data in descending order
•removing the legend
•adding a title
•labeling the axes

my_plot = sales_totals.sort_values(by='ext price', ascending=False).plot(kind='bar',legend=None,title="Total Sales by Customer")
my_plot.set_xlabel("Customers")
my_plot.set_ylabel("Sales ($)")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
