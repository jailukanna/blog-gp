---
title: mysql example sql to xlsx (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'sql to xlsx'

Functions in program: 
* `def query():`

Modules used in program: 
* `import pandas as pd`
* `import mysql.connector`
* `import os`
* `import logging`

## python sql to xlsx

Python mysql example: sql to xlsx

```python
# -*- coding: utf-8 -*-
import logging
import os

import mysql.connector
import pandas as pd

logging.basicConfig(level=logging.DEBUG)


def query():
    output_dir = os.path.join(os.path.dirname(os.path.abspath(__file__)), 'out')
    os.makedirs(output_dir, exist_ok=True)
    xlsx_filepath = os.path.join(output_dir, 'foo.xlsx')
    writer = pd.ExcelWriter(xlsx_filepath)

    mydb = mysql.connector.connect(
        host="localhost",
        user="root",
        passwd="1",
        database="test"
    )

    mycursor = mydb.cursor()

    mycursor.execute('show tables')
    tables = mycursor.fetchall()  # list

    for table in tables:
        table = table[0]
        logging.info('table {}'.format(table))

        mycursor.execute("SELECT * FROM " + table)

        columns = [desc[0] for desc in mycursor.description]
        myresult = mycursor.fetchall()  # list
        logging.info(myresult)

        df = pd.DataFrame(myresult, columns=columns)

        df.to_excel(writer, sheet_name=table)

    writer.save()


if __name__ == '__main__':
    query()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
