---
title: matplotlib example Scatter plot bokeh (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Scatter plot bokeh'


Modules used in program: 
* `import pandas as pd`

## python Scatter plot bokeh

Python matplotlib example: Scatter plot bokeh

```python
from bokeh.plotting import figure, show, output_file
import pandas as pd

data = pd.read_csv('iris.csv')

colormap = {'setosa': 'red', 'versicolor': 'green', 'virginica': 'blue'}
colors = [colormap[x] for x in data['species']]

p = figure(title = "Iris Morphology")
p.xaxis.axis_label = 'Petal Length'
p.yaxis.axis_label = 'Petal Width'

p.circle(data["petal_length"], data["petal_width"],
        color=colors, fill_alpha=0.2, size=10)

output_file("iris.html", title="iris.py example")

show(p)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
