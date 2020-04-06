---
title: mysql example Chapter8 1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Chapter8 1'


Modules used in program: 
* `import re`
* `import numpy as np`
* `import pandas as pd`

## python Chapter8 1

Python mysql example: Chapter8 1

```python
import pandas as pd
import numpy as np
import re

pd.options.display.max_rows=1000

data=pd.Series(np.random.randn(9), index=[['a','a','a','b','b','c','c','d','d'],[1,2,3,1,3,1,2,2,3]])
print(data)
print(data.index)
print(data.loc['b'])
print(data[['b','d']])
print(data[:,2])
print(data.unstack())

frame = pd.DataFrame(np.arange(12).reshape((4, 3)),
                     index=[['a', 'a', 'b', 'b'], [1, 2, 1, 2]],
                     columns=[['Ohio', 'Ohio', 'Colorado'],
                              ['Green', 'Red', 'Green']])
print(frame)
frame.index.names=['key1','key2']
frame.columns.names=['state','color']
print('frame: \n',frame)
print('frame com key hierarquy mudadas : \n',frame.swaplevel('key2','key1'))
print(frame.sum(level='key2'))
frame.replace()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
