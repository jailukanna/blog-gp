---
title: mysql example python-mysql-demo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'python-mysql-demo'


Modules used in program: 
* `import mysql.connector, datetime`
* `import ConfigParser as cp`
* `import sys`

## python python-mysql-demo

Python mysql example: python-mysql-demo

```python
import sys
import ConfigParser as cp
import mysql.connector, datetime
from mysql.connector import errorcode

props = cp.RawConfigParser()
props.read("../resources/application.properties")
try:
    env = sys.argv[1]
    username = props.get(env, "db_username")
    password = props.get(env, "db_password")
    hostname = props.get(env, "db_hostname")
    database = props.get(env, "db_name")
    cnx = mysql.connector.connect(user=username, password=password,
                                  host=hostname,
                                  database=database)

    cursor = cnx.cursor()
    # query = ("select * from orders limit 10")
    # cursor.execute(query)

    # for i in cursor:
    #     print(i)

    query = ("SELECT first_name, last_name, " +
      "case when commission_pct is null then 'Not Eligible' else " +
      "salary * commission_pct end commission_amount FROM employees")

    cursor.execute(query)

    # for i in cursor:
    #     print(i)

    l = list(cursor)
    for i in l: print("first_name:" + i[0] + ";" +
                      "last_name:" + i[1] + ";" +
                      "commission_amount:" + i[2])
    cursor.close()

except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with your user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cnx.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
