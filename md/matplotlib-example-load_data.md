---
title: matplotlib example load data (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'load data'


Modules used in program: 
* `import scipy.optimize #Package we'll use to optimize`
* `import pandas #Package to data manipulation`
* `import matplotlib.pyplot as plt #From matplotlib package we import pyplot for plots`
* `import numpy as np #Package we'll use for numerical calculations`

## python load data

Python matplotlib example: load data

```python
import numpy as np #Package we'll use for numerical calculations
import matplotlib.pyplot as plt #From matplotlib package we import pyplot for plots
import pandas #Package to data manipulation
import scipy.optimize #Package we'll use to optimize
plt.style.use('seaborn-colorblind') #This is a pyplot style (optional)

'''Load the data into a pandas series with the name wine_sales'''
time_series = pandas.Series.from_csv("wine_sales.csv", header=0)

P=12 #number of seasonal periods in a cycle


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
