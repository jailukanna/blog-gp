---
title: matplotlib example pandas (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'pandas'


Modules used in program: 
* `import pandas as pd`

## python pandas

Python matplotlib example: pandas

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import pandas as pd

df = pd.read_csv("datos.csv",nrows=10)
df.head()
df.head(10)
df.tail()
df.sample(frac=2)
pf = df.sample(frac=2)
df.columns
df.dtypes
df.values
df2 = df.head(3)
df["Columna"].head()

df[10:2]

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
