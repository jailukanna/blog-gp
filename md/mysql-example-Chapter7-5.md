---
title: mysql example Chapter7-5 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Chapter7-5'


Modules used in program: 
* `import numpy as np`
* `import pandas as pd`

## python Chapter7-5

Python mysql example: Chapter7-5

```python
import pandas as pd
import numpy as np
from statsmodels.stats.tests.test_multi import NA

data=pd.DataFrame({'food': ['bacon','pulled pork','bacon','Pastrami', 'corned beef', 'Bacon','pastrami', 'honey ham','nova lox'],'ounces': [4,3,12,6,7.5,8,3,5,6]})
print(data)
meat_to_animal={'bacon':'pig','pulled pork':'pig','pastrami': 'cow','corned beef':'cow','honey ham':'pig','nova lox':'salmon'}
print(meat_to_animal)
#print(data.duplicated())
#aqui esoy haciendo el diccionario meat_to_animal en minuscula

lowercased=data['food'].str.lower()
print(lowercased)

p=data['animal']=lowercased.map(meat_to_animal) #adicionando la columna animal al dataframe
print(p)
print('data: \n',data)
print(data.duplicated(['food','animal']))
print(data.drop_duplicates(['food','animal']))#eliminando las filas duplicadas a partir das columnas food y animal

serie=data.drop_duplicates(['food','animal'])
print('serie: \n',serie)
new=serie.reindex(index=np.arange(len(serie)))
print('new: \n',new)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
