---
title: mysql example py trouble reading tabula (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'py trouble reading tabula'


Modules used in program: 
* `import pandas`
* `import os`
* `import tabula`

## python py trouble reading tabula

Python mysql example: py trouble reading tabula

```python
import tabula
import os
from tabula import read_pdf
import pandas


#expected_csv = 'C:\Users\abhishek chandel\python program\x.csv'
df =tabula.read_pdf('C:\ghr.pdf',pages="all", multiple_tables=True)
print(df)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
