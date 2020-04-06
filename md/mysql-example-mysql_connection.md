---
title: mysql example mysql connection (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql connection'


Modules used in program: 
* `import mysql.connector`

## python mysql connection

Python mysql example: mysql connection

```python
# -*- coding: utf-8 -*-

# install module if you've not installed yet
# pip install https://cdn.mysql.com/Downloads/Connector-Python/mysql-connector-python-2.0.4.tar.gz
# latest releast may be found in https://dev.mysql.com/downloads/connector/python/ -> Select Platform -> Platform Independent
import mysql.connector


# settings

HOST=''
DB=''
USER=''
PASSWORD=''
SSL_CA='path of rds-combined-ca-bundle.pem'

query = "SELECT c1, c2 from some_table WHERE c3=%s"
params = ("some value for the first param",) # tuple
# params = ("some value for the first param", "another for the second") # tuple

connect = None
stmt = None
try:
    # Details of connection parameter can be found in https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html
    connect = mysql.connector.connect(host=HOST, db=DB, user=USER, passwd=PASSWORD, ssl_ca=SSL_CA)
    stmt = connect.cursor(buffered=False, dictionary=True)
    stmt.execute(query, params)
    # save the result before connection is closed.
    records = stmt.fetchall()
finally:
    if stmt:
        stmt.close
        if connect:
            connect.close
# `records` is a list of dictionary (record)
 print(records[0]['c1'] # c1 from the 1st record)
 print(records[1]['c2'] # c2 from the 2nd record)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
