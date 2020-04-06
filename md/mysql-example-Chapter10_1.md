---
title: mysql example Chapter10 1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Chapter10 1'


Modules used in program: 
* `import seaborn as sns`
* `import numpy as np`
* `import pandas as pd`

## python Chapter10 1

Python mysql example: Chapter10 1

```python
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
from statsmodels.stats.tests.test_multi import NA
from numpy.random import randn
import seaborn as sns

#MERGE
print('=====================PRACTICANDO EL MODO DE JUNCION MERGE=================================')
a=int(input('escriba un numero: '))
print(a)
e=pd.DataFrame( {'data':np.random.randn(a),'key':range(a)})
e.index.names=['a']
#print(e)
f=pd.DataFrame( {'data':np.random.randn(a),'key':range(a)})
f.index.names=['a']

#print(e)
#print(f)
g=pd.merge(e,f, on='key', how='outer')
print(g)
#print(g.dropna())
#print(g[['key','data_x','data_y']].iloc[20])
print(g['data_x'].describe())
print(round(g[:10]['data_x'].sum()),3)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
