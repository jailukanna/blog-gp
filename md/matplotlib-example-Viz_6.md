---
title: matplotlib example Viz 6 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Viz 6'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import seaborn as sns`
* `import pandas as pd`

## python Viz 6

Python matplotlib example: Viz 6

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

sns.set(style='white', color_codes=True)
iris = pd.read_csv("iris.csv")

sns.boxplot(x="species", y="petal_length", data=iris)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
