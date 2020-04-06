---
title: mysql example tabula%2520read3 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'tabula%2520read3'


Modules used in program: 
* `import pandas as pd`
* `import os`
* `import tabula`

## python tabula%2520read3

Python mysql example: tabula%2520read3

```python
import tabula
import os
import pandas as pd
 
folder = r'Users\abhishek chandel\python programs'
paths = [folder + fn for fn in os.listdir(folder) if fn.endswith('.pdf')]
for path in paths:
    df = tabula.read_pdf(path, encoding = 'cp932', pages = 'all', spreadsheet = True)
    path = path.replace('pdf', 'csv')
    df.to_csv(path, index = False)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
