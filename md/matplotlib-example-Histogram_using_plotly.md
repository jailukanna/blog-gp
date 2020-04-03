---
title: matplotlib example Histogram using plotly (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Histogram using plotly'


Modules used in program: 
* `import pandas as pd`
* `import plotly.graph_objs as go`
* `import plotly.plotly as py`

## python Histogram using plotly

Python matplotlib example: Histogram using plotly

```python
import plotly.plotly as py
import plotly.graph_objs as go
import pandas as pd

iris = pd.read_csv("iris.csv")

data = [go.Histogram(x=iris['sepal_length'].tolist())]

layout = go.Layout(title='Iris Dataset - Sepal.Length',
xaxis=dict(title='Sepal.Length'),
yaxis=dict(title='Count')
)

fig = go.Figure(data=data, layout=layout)
py.iplot(fig)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
