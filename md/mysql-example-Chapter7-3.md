---
title: mysql example Chapter7-3 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Chapter7-3'


Modules used in program: 
* `import numpy as np`
* `import pandas as pd`

## python Chapter7-3

Python mysql example: Chapter7-3

```python
import pandas as pd
import numpy as np
from statsmodels.stats.tests.test_multi import NA

df=pd.DataFrame(np.random.rand(7,7), index=np.arange(7))
df.iloc[0:4,1:5]=NA
df.iloc[5:,5:7]=NA
print(df)
print('indice completo: \n',df.dropna())

'''de=df.iloc[4]
print('de: \n',de)
print('=================================================================================')
dg=df[3]
print('df: \n',dg)'''
print('=================================================================================')
dh=df.fillna({1:df[1].median(),2: df[2].mean(),3:df[3].mean(), 4:df[4].median(),5:df[5].mean(),6:df[6].median()})
print('dh: \n',dh)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
