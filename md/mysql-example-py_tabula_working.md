---
title: mysql example py tabula working (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'py tabula working'


Modules used in program: 
* `import pandas as pd`
* `import os`
* `import tabula`

## python py tabula working

Python mysql example: py tabula working

```python
import tabula
import os
from tabula import read_pdf
import pandas as pd


#expected_csv = 'C:\Users\abhishek chandel\python program\x.csv'
df =tabula.read_pdf('C:\ghr.pdf', encoding="cp932",pages="all", multiple_tables=True)


Regional_data = df[0]
FREQ = pd.DataFrame(df[1])
PSPS = pd.DataFrame(df[2])
ICTE = pd.DataFrame(df[3])
IER =  pd.DataFrame(df[4])
GEN_OUT = pd.DataFrame(df[5])
SOURCE_GEN = pd.DataFrame(df[6])
#print(PSPS)


#now that we have PSPS we want to improve this file
for x in range (3,42):
 states = PSPS[1][x]
 print(states)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
