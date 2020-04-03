---
title: matplotlib example plot quadratic curve swap rate interpolate (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot quadratic curve swap rate interpolate'


Modules used in program: 
* `import matplotlib`
* `import matplotlib.pyplot as plt`
* `import sys`
* `import time`
* `import datetime as dt`
* `import pandas as pd`
* `import numpy as np`
* `import xlwings as xw`

## python plot quadratic curve swap rate interpolate

Python matplotlib example: plot quadratic curve swap rate interpolate

```python
import xlwings as xw
import numpy as np
import pandas as pd
import datetime as dt
import time
import sys

# Matplotlib

%matplotlib inline
from scipy.interpolate import interp1d
import matplotlib.pyplot as plt
import matplotlib

# Swap rate example
years = [1, 2, 3, 4, 5, 7, 10, 30]
swap_rate = [0.0079, 0.0094, 0.0107, 0.0119,
             0.013, 0.0151, 0.0174, 0.022]
years_new = np.linspace(1, 30, num=150)
interpolate = interp1d(years, swap_rate, kind='quadratic')

"""
Example: update it by changing mannually

years = [1, 2, 3, 4, 5, 7, 10]
swap_rate = [0.0079, 0.0094, 0.0107, 0.0119,
             0.013, 0.0151, 0.0174]
years_new = np.linspace(1, 10, num=10)
interpolate = interp1d(years, swap_rate, kind='quadratic')
"""

fig = plt.figure(figsize=(6, 4))
swaprate_plot = plt.plot(years, swap_rate, 'o',
                         years_new, interpolate(years_new), '-')

wb = xw.Book()
sheet = wb.sheets[0]

plot = sheet.pictures.add(fig, name='SwapRate', update=True)

# Fine Tuning - manipulate properties after adding the picture
plot.height = plot.height / 2
plot.width = plot.width / 2

# Fine Tuning - create new chart underneath
width, height = fig.get_size_inches()
dpi = fig.get_dpi()
sheet.pictures.add(fig, name='SwapRate2', update=True,
                   left=sheet.range('A25').left, top=sheet.range('A25').top,
                   width=width * dpi / 2, height=height * dpi / 2)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
