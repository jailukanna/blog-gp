---
title: mysql example testing mysql queries (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'testing mysql queries'

Functions in program: 
* `def get_mysql_data(q,cols):`

Modules used in program: 
* `import pandas as pd`
* `import numpy as np`
* `import mysql.connector`
* `import json`

## python testing mysql queries

Python mysql example: testing mysql queries

```python
import json
import mysql.connector
import numpy as np
import pandas as pd
from pandas import Series, DataFrame

# p as path, c as connection
with open('/Users/path/to/database/keys/prod_db.json') as p:
    c = json.load(p)
    p.close()

query_users = """
SELECT users.id,users.attributes
FROM users
WHERE users.created_at > '2013-12-31 23:59:59'
"""

# accepts query and list of column names, returns dataframe
def get_mysql_data(q,cols):
    cnx = mysql.connector.connect(user=c['user'],password=c['password'],host=c['host'],database=c['dbname'])
    cursor = cnx.cursor()
    cursor.execute(q)
    data = cursor.fetchall()
    df = pd.DataFrame(data,columns=cols)
    cursor.close()
    cnx.close()
    return df

users = get_mysql_data(query_users,['user_id','attributes'])


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
