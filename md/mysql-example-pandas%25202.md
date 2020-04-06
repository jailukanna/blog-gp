---
title: mysql example pandas%25202 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'pandas%25202'


Modules used in program: 
* `import pandas as pd`
* `import tabula`

## python pandas%25202

Python mysql example: pandas%25202

```python

import tabula
from tabula import read_pdf
import pandas as pd




df =tabula.read_pdf('C:\ghr.pdf', encoding="cp932",pages="all", multiple_tables=True)
Regional_data = df[0]
FREQ = pd.DataFrame(df[1])
PSPS = pd.DataFrame(df[2])
ICTE = pd.DataFrame(df[3])
IER =  pd.DataFrame(df[4])
GEN_OUT = pd.DataFrame(df[5])
SOURCE_GEN = pd.DataFrame(df[6])











```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
