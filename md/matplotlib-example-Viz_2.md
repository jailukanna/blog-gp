---
title: matplotlib example Viz 2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Viz 2'


Modules used in program: 
* `import pandas as pd`
* `import matplotlib.pyplot as plt`

## python Viz 2

Python matplotlib example: Viz 2

```python
import matplotlib.pyplot as plt
import pandas as pd

iris = pd.read_csv('iris.csv')

p_length = iris['petal_length'].tolist()

bins = [0,1,2,3,4,5,6,7,8,9]

plt.hist(p_length, bins, histtype = 'bar', rwidth=0.8)

plt.xlabel("Petal Length")
plt.ylabel("Count")
plt.title("Petal Length Distribution")
plt.legend()
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
