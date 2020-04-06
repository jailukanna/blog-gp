---
title: mysql example write to db (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'write to db'


Modules used in program: 
* `import mysql.connector.pooling`
* `import pandas as pd`

## python write to db

Python mysql example: write to db

```python
import pandas as pd
import mysql.connector.pooling

start_cred = {
  'db_user': <db_user>,
  ...
}
cnx =  mysql.connector.pooling.MySQLConnectionPool(**start_cred)
data = pd.read_csv('filename')

# query to fill in
QUERY = "insert_query.sql"
# Example:
# INSERT INTO table (id, item)
# VALUES (%(id)s, %(item)s);

with open(QUERY) as fh:
    query = fh.read()

try:
    con = cnx.get_connection()
    cur = con.cursor()
    for _, row in data.iterrows():
        param = {
            'id': row['id'],
            'cluster'  : row['item'],
            }
        cur.execute(query, param)
    con.commit()  # to apply changes
finally:
    con.close()
    cur.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
