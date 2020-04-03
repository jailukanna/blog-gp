---
title: matplotlib example confirmedindo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'confirmedindo'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import pandas as pd`

## python confirmedindo

Python matplotlib example: confirmedindo

```python
import pandas as pd
import matplotlib.pyplot as plt

df_confirmed=pd.read_csv('./time_series_covid19_confirmed_global.csv')

indo = df_confirmed['Country/Region']=='Indonesia'
df_conf_indo=df_confirmed[indo]
df_conf_indo=df_conf_indo.drop(columns=['Province/State','Country/Region','Lat','Long'])
df_conf_indo=df_conf_indo.transpose()
df_conf_indo.columns=['indonesia']

plt.plot(df_asean.indonesia)
plt.legend(['indonesia'],loc="upper left")
plt.xticks(rotation=90)
plt.figure(figsize=(10,5))
plt.show

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
