---
title: mysql example pandas to mysql example (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'pandas to mysql example'


Modules used in program: 
* `import pandas as pd`
* `import mysql.connector as m`

## python pandas to mysql example

Python mysql example: pandas to mysql example

```python
import mysql.connector as m
import pandas as pd


if __name__ == '__main__':
    mysql_creds = {
        'host': '127.0.0.1',
        'database': 'test',
        'user': 'root',
        'password': ''
    }
    try:
        connection = m.connect(**mysql_creds)
        cursor = connection.cursor()
    except m.Error as err:
        raise SystemExit('Cant readh MySQL')

    data = pd.read_csv('data.csv')
    for index, row in data.iterrows():
        q = 'insert into testtable (name, sex, age, weight, height) VALUES (%s, %s, %s, %s, %s)'
        cursor.execute(q, [row.get(k) for k in row.keys()])

    connection.commit()
    cursor.close()
    connection.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
