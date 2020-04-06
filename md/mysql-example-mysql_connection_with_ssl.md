---
title: mysql example mysql connection with ssl (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql connection with ssl'


Modules used in program: 
* `import mysql.connector`

## python mysql connection with ssl

Python mysql example: mysql connection with ssl

```python
import mysql.connector
from mysql.connector.constants import ClientFlag
from mysql.connector import Error

config = {
    'user': 'ssluser',
    'password': 'Pass@123#',
    'host': '192.168.1.77',
    'database':'world',
    'client_flags': [ClientFlag.SSL],
    'ssl_ca': '/home/sh/mysql-ssl/ca-cert.pem',
    'ssl_cert': '/home/sh/mysql-ssl/client-cert.pem',
    'ssl_key': '/home/sh/mysql-ssl/client-key.pem',
}

try:

    cnx = mysql.connector.connect(**config)
except Error as err:
    print(err)

cur = cnx.cursor(buffered=True)
query = "select * from city"
b = True
while b:
    cur.execute(query)
    for a in cur:
        print(a)
#cur.execute("SHOW STATUS LIKE 'Ssl_cipher'")

cur.close()
cnx.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
