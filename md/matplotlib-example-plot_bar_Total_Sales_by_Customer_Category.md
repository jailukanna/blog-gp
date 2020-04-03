---
title: matplotlib example plot bar Total Sales by Customer Category (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot bar Total Sales by Customer Category'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import pandas as pd`

## python plot bar Total Sales by Customer Category

Python matplotlib example: plot bar Total Sales by Customer Category

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

customers = sales[['name','category','ext price','date']]

category_group=pd.crosstab(customers.name, customers.category, values=customers['ext price'], aggfunc='sum')

my_plot = category_group.plot(kind='bar',stacked=True,title="Total Sales by Customer", figsize=(9, 7))
my_plot.set_xlabel("Customers")
my_plot.set_ylabel("Sales")
my_plot.legend(["Total","Belts","Shirts","Shoes"], loc=9, ncol=4)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
