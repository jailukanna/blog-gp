---
title: mysql example Chapter7 10 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Chapter7 10'


Modules used in program: 
* `import numpy as np`
* `import pandas as pd`

## python Chapter7 10

Python mysql example: Chapter7 10

```python
import pandas as pd
import numpy as np

data=pd.DataFrame(np.arange(12).reshape(3,4), index=['Ohio','Colorado','New York'], columns=['one', 'two','three','four'])

print(data)
print('======================================================')
transform= lambda x: x[:4].upper()

data.index=data.index.map(transform)
print(data)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
