---
title: matplotlib example Viz 5 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Viz 5'


Modules used in program: 
* `import pandas as pd`
* `import matplotlib.pyplot as plt`

## python Viz 5

Python matplotlib example: Viz 5

```python
import matplotlib.pyplot as plt
import pandas as pd

iris = pd.read_csv('iris.csv')

for n in range(0,150):
 if iris['species'][n] == 'setosa':
    plt.scatter(iris['sepal_length'][n],iris['sepal_width'][n],color='red')
    plt.xlabel('sepal_length')
    plt.ylabel('sepal_width')
    
 elif iris['species'][n] == 'versicolor':
    plt.scatter(iris['sepal_length'][n],iris['sepal_width'][n],color='blue')
    
 else:
    plt.scatter(iris['sepal_length'][n],iris['sepal_width'][n],color='green')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
