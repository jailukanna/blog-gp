---
title: mysql example Chapter7 11 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Chapter7 11'


Modules used in program: 
* `import numpy as np`
* `import pandas as pd`

## python Chapter7 11

Python mysql example: Chapter7 11

```python
import pandas as pd
import numpy as np

ages=np.arange(100)
bins=[0,25,35,60,100]
group_names=['Youth','YoungAdult','MiddleAged','Senior']
cats=pd.cut(ages,bins=bins,labels=group_names)
print(cats)
fr=pd.value_counts(cats)
frame=pd.DataFrame(fr, columns=['cantidad'])

frame2=frame.rename(columns=str.upper)
print(frame2.sort_values)

data=pd.DataFrame(np.random.randn(1000,4))
print(data.describe())
col=data[2]
print(data[(np.abs(data)>3).any(1)])





```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
