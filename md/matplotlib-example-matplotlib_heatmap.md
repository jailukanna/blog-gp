---
title: matplotlib example matplotlib heatmap (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib heatmap'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib as mpl`
* `import matplotlib.cm as cm`
* `import matplotlib.ticker as ticker`

## python matplotlib heatmap

Python matplotlib example: matplotlib heatmap

```python
import matplotlib.ticker as ticker
import matplotlib.cm as cm
import matplotlib as mpl

import matplotlib.pyplot as plt
%matplotlib inline


fig = plt.figure()
fig, ax = plt.subplots(1,1, figsize=(12,12))
heatplot = ax.imshow(flight_matrix, cmap='BuPu')
ax.set_xticklabels(flight_matrix.columns)
ax.set_yticklabels(flight_matrix.index)

tick_spacing = 1
ax.xaxis.set_major_locator(ticker.MultipleLocator(tick_spacing))
ax.yaxis.set_major_locator(ticker.MultipleLocator(tick_spacing))
ax.set_title("Heatmap of Flight Density from 1949 to 1961")
ax.set_xlabel('Year')
ax.set_ylabel('Month')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
