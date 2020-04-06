---
title: mysql example read queries file v0.1.1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'read queries file v0.1.1'

Functions in program: 
* `def read_sql_queries_file(file, con, replace_dict=None, ignore_list=['/* MANDATORY */', '/* OPTIONALS */', '/* OPTIONAL */']):`
* `def mysql_con(host, database, user, password):`

Modules used in program: 
* `import db_config as config  # Your DB credentials`
* `import mysql.connector as sql`
* `import pandas as pd`

## python read queries file v0.1.1

Python mysql example: read queries file v0.1.1

```python
import pandas as pd
import mysql.connector as sql
from future.utils import iteritems
import db_config as config  # Your DB credentials

# Local functions
def mysql_con(host, database, user, password):
    """
    Crea conector de MySQL.
    """
    return sql.connect(host=host, database=database, user=user, password=password)

def read_sql_queries_file(file, con, replace_dict=None, ignore_list=['/* MANDATORY */', '/* OPTIONALS */', '/* OPTIONAL */']):
    """
    Lee un archivo .sql y lo carga en un diccionario de dataframes.
    Cada query debe estar separada por ';' (punto y coma) y el nombre debe estar al principio de la query en /*NAME: nombre_query */
    """
    dfs_dict = {}
    with open(file, 'r') as f:
        query = f.read()
        if bool(ignore_list) and isinstance(ignore_list, list):
            for ignore in ignore_list:
                query = query.replace(ignore, '')
        if bool(replace_dict) and isinstance(replace_dict, dict):
            for (key, value) in iteritems(replace_dict):
                query = query.replace(key, value)
        queries_list = query.split(';')
        for query in queries_list:
            if 'NAME:' in query:
                name = query.strip().splitlines()[0].replace('/*NAME:', '').replace('*/', '').strip()
                try:
                    dfs_dict[name] = pd.read_sql_query(query, con=con)
                     print('{} query added! c:'.format(name))
                except:
                     print('Oops! {} wrong query! :c'.format(name))
                    pass
    return dfs_dict


# Credentials
credentials_dict = {'host': config.host, 'database': 'database_name', 'user': config.user, 'password': config.password}

# Example
df_dict = read_sql_querys_file(file='dummy_querys.sql', con=mysql_con(**credentials_dict), replace_dict={':idSimulacion': simulacion})

# Dictionary's values as local variables
locals().update(dfs_dict)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
