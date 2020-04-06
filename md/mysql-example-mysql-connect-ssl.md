---
title: mysql example mysql-connect-ssl (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql-connect-ssl'


Modules used in program: 
* `import mysql.connector;`

## python mysql-connect-ssl

Python mysql example: mysql-connect-ssl

```python
import mysql.connector;

conn = mysql.connector.connect(host="127.0.0.1", 
 port=3306,
    user="ssl_user",
    passwd="password",
    db="db",
    ssl_ca="/etc/ssl/mysql/ca.pem",
    ssl_key="/etc/ssl/mysql/client-key.pem",
    ssl_cert="/etc/ssl/mysql/client-cert.pem"
);

cursor = conn.cursor()
region = "us"
query = "SELECT name FROM realms WHERE region='%s'" % (region)
cursor.execute(query)

for result in cursor.fetchall():
 realm = result[0]
 print("Realm: ", str(realm))

cursor.close()
conn.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
