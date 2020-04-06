---
title: mysql example Chapter7-4 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Chapter7-4'


Modules used in program: 
* `import numpy as np`
* `import pandas as pd`

## python Chapter7-4

Python mysql example: Chapter7-4

```python
import pandas as pd
import numpy as np
from statsmodels.stats.tests.test_multi import NA

df=pd.DataFrame(np.random.randn(6,3))
df.iloc[2:,1]=NA
df.iloc[4:,2]=NA

print(df)
print(df.fillna(method='ffill', limit=1))
e=df.fillna(method='ffill', limit=2)
f=e.fillna({1:e[1].mean(),2:e[2].median()})
print('f: \n',f)

print('=========REMOVIENDO DUPLICADOS===================================================')

k=pd.DataFrame({'k1':['one','two']*3+['two'],'k2':[1,1,2,3,3,4,4]})
print(k)#aqui k1 y k2 serian las keys doel diccionario y asi mismo serian los nombres de las columnas
print(k.duplicated())
r=k.drop_duplicates()
print(r)

k['v1']=range(len(k))
print(k)
f=k.drop_duplicates(['k1','k2'], keep='last')
print(k)
print(f)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
