---
title: mysql example pandas MySQL Example (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'pandas MySQL Example'


Modules used in program: 
* `import mysql.connector`
* `import pandas as pd`
* `import os`

## python pandas MySQL Example

Python mysql example: pandas MySQL Example

```python
import os

import pandas as pd
import mysql.connector
from sqlalchemy import create_engine

# ====== Connection ====== #
# Connecting to mysql by providing a sqlachemy engine
engine = create_engine('mysql+mysqlconnector://myhu:password@192.168.68.102/dbname', echo=True)

list_hello = ['hello1', 'hello2']
list_world = ['world1', 'world2']
df = pd.DataFrame(data={'hello': list_hello, 'world': list_world})

# engine.execute("CREATE DATABASE dbname") #create db


print(engine.table_names())

df.to_sql(name='helloworld', con=engine, if_exists='replace')

print(df)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
