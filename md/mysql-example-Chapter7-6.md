---
title: mysql example Chapter7-6 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Chapter7-6'


Modules used in program: 
* `import numpy as np`
* `import pandas as pd`

## python Chapter7-6

Python mysql example: Chapter7-6

```python
import pandas as pd
import numpy as np
from statsmodels.stats.tests.test_multi import NA
data=pd.Series([1.,-999.,2.,-999.,-1000.,3.])
data2=np.arange(len(data))


print(data)
print(list(zip(data,data2)))
cosa=list(zip(data,data2))
frame=pd.DataFrame(cosa, columns=['data','data2'], index=['a','b','c','d','e','f'])
print(frame)
print(frame.sort_values(by='data'))
d=frame.replace({-999:0, -1000:0.5})
print(d)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
