---
title: matplotlib example Viz 4 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Viz 4'


Modules used in program: 
* `import pandas as pd`
* `import plotly.graph_objs as go`
* `import plotly.plotly as py`

## python Viz 4

Python matplotlib example: Viz 4

```python
import plotly.plotly as py
import plotly.graph_objs as go
import pandas as pd

iris = pd.read_csv("iris.csv")

data = [go.Bar(x=['setosa','versicolor','virginica'],
y=[iris.loc[iris['species']=='setosa'].shape[0],
   iris.loc[iris['species']=='versicolor'].shape[0],iris.loc[iris['species']=='virginica'].shape[0]]

)]


layout = go.Layout(title='Iris Dataset - Species',
xaxis=dict(title='Iris Dataset - Species'),
yaxis=dict(title='Count')
)

fig = go.Figure(data=data, layout=layout)
py.iplot(fig)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
