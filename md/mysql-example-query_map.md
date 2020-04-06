---
title: mysql example query map (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'query map'

Functions in program: 
* `def basic_data(data):`

Modules used in program: 
* `import mysql.connector`
* `import os`
* `import pandas as pd`

## python query map

Python mysql example: query map

```python
import pandas as pd
import os
import mysql.connector
def basic_data(data):
    query= data[0]
    cursor = data[1]

    cursor.execute(query)
    df =  pd.DataFrame(cursor.fetchall(),columns=cursor.column_names)

    return df

query1 = """ select * from tbl1 """
query2 = """ select * from tbl2 """
queryn = """ select * from tbln """

queries = [query1, query2, queryn]

creds = os.environ['MYSQL_CREDS'].split('@#$')
cnx = mysql.connector.connect(host=creds[0],user=creds[1],password=creds[2],database=creds[3])
cursor = cnx.cursor()

map_queries = list(zip(queries, len(queries)*[cursor]))
get_data = list(map(basic_data, map_queries))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
