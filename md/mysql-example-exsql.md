---
title: mysql example exsql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'exsql'


Modules used in program: 
* `import argparse`
* `import os`
* `import mysql.connector`
* `import pandas as pd`
* `import argparse`

## python exsql

Python mysql example: exsql

```python
import argparse
import pandas as pd
import mysql.connector
from sqlalchemy import create_engine
from string import Template
import os

import argparse

parser = argparse.ArgumentParser()
parser.add_argument("-u", "--user", help="mysql user", nargs='?', const='')
parser.add_argument("-p", "--password", help="mysql password", nargs='?', const='')
parser.add_argument("-e", "--excel", help="Excel file to table-ize.\nIf the file does not exist try to create from table")
parser.add_argument("database", help="database to use")
parser.add_argument("table", help="table to create/export")

args = parser.parse_args()

excel_file = os.path.abspath(args.excel)

connection_template = Template('mysql+mysqlconnector://$user:$password@localhost/$database')
connection_string = connection_template.substitute(user=args.user, password=args.password, database=args.database)
print("Connecting to " + connection_string)
engine = create_engine(connection_string, echo=False)

if os.path.exists(excel_file):
    print("populating " + args.table)
    records = pd.read_excel(excel_file)
    records.to_sql(name = args.table, con=engine, if_exists='append', index=False, chunksize=2)
else:
    print("exporting " + args.table + " to " + excel_file)
    records = pd.read_sql_table(args.table, engine)
    records.to_excel(excel_file, sheet_name=args.table)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
