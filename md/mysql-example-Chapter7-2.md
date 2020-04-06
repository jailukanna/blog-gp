---
title: mysql example Chapter7-2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Chapter7-2'


Modules used in program: 
* `import numpy as np`
* `import pandas as pd`

## python Chapter7-2

Python mysql example: Chapter7-2

```python
import pandas as pd
import numpy as np
from statsmodels.stats.tests.test_multi import NA

df=pd.DataFrame(np.random.randn(7,3))
print(df)

df.iloc[:4,1]=NA #aqui estoy haceindo cero los primeros cuatro elementos de la columna 1.
df.iloc[:2,2]=NA #aqui esttoy haciendo 0 los primeros dos elementos de la columna 2.
print(df)
print(df.dropna()) #aqui estoy imprimiendo apenas las filas cuyos daos estan completos.

print(df.fillna({1:df[1].mean(),2:df[2].median()}))
#aqui estoy imprimiendo la accion de llenar los datos ausentes de la columna 1 con media de la columna 1 y
#  llenando con la mediana de la columna 2 los dados ausentes de la columna 2.






```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
