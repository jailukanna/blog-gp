---
title: mysql example json-sql-wpython (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'json-sql-wpython'

Functions in program: 
* `def connection():`
* `def flatten_json(y):`

Modules used in program: 
* `import csv`
* `import json`
* `import mysql.connector`

## python json-sql-wpython

Python mysql example: json-sql-wpython

```python
import mysql.connector
import json
import csv

def flatten_json(y):
    out = {}

    def flatten(x, name=''):
        if type(x) is dict:
            for a in x:
                flatten(x[a], name + a + '_')
        elif type(x) is list:
            i = 0
            for a in x:
                flatten(a, name + str(i) + '_')
                i += 1
        else:
            out[name[:-1]] = x

    flatten(y)
    return out

def connection():
    db = mysql.connector.connect(user='user', password='password',
                                 host='host',
                                 database='db')

    cursor = db.cursor()

    query = ('SELECT id, data FROM table')

    cursor.execute(query)

    columns = cursor.description
    result = [{columns[index][0]:column for index, column in enumerate(value)} for value in cursor.fetchall()]
    return result

temp = connection()
new_temp = []
for ele in temp:
    id = ele["id"]
    to_conv = json.loads(ele["data"])
    to_conv = flatten_json(to_conv)
    to_conv["id"] = id
    new_temp.append(to_conv)


keys = new_temp[0].keys()
with open('output.csv', 'w') as output_file:
    dict_writer = csv.DictWriter(output_file, keys)
    dict_writer.writeheader()
    dict_writer.writerows(new_temp)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
