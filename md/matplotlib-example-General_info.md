---
title: matplotlib example General info (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'General info'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import pandas as pd`

## python General info

Python matplotlib example: General info

```python
import pandas as pd
import matplotlib.pyplot as plt
tax_filers = pd.read_csv('/Users/ngocphan/PycharmProjects/Personal_tax/number_of_filers.csv')
columns = tax_filers.columns[1:6]
y_pos = list(tax_filers.iloc[14, 1:6])

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
