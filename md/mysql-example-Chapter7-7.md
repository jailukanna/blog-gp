---
title: mysql example Chapter7-7 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Chapter7-7'


Modules used in program: 
* `import numpy as np`
* `import pandas as pd`

## python Chapter7-7

Python mysql example: Chapter7-7

```python
import pandas as pd
import numpy as np
from statsmodels.stats.tests.test_multi import NA
from matplotlib import pyplot as plt
from Functions_Alejandro import contador_positivos
pd.set_option('display.max_columns',10)
pd.set_option('display.max_rows',1000)
a=np.random.randn(1000,10)
print(a)
b=pd.DataFrame(a,index=np.arange(len(a)))

print(b)
c=b[5]
#print(c)
g=contador_positivos(c)
n=((len(a)-(len(a)-g))/len(a))*100
print(g)
print('positivos: ',round(n,4),'%')


pattern=r'[A-Z0-9._%]+@[A-Z0-9.-]+\.[A-Z]{2,4}'
print(pattern)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
