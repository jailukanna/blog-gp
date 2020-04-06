---
title: mysql example mysql-drivers-test (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql-drivers-test'

Functions in program: 
* `def doQuery( conn ) :`

Modules used in program: 
* `import mysql.connector`
* `import pymysql`
* `import MySQLdb`

## python mysql-drivers-test

Python mysql example: mysql-drivers-test

```python
#!/usr/bin/python

hostname = '127.0.0.1'
username = 'root'
password = 'root'
database = 'schooldb'

# Simple routine to run a query on a database and print(the results:)
def doQuery( conn ) :
    cur = conn.cursor()

    cur.execute( "SELECT fname, lname FROM students" )

    for firstname, lastname in cur.fetchall() :
        print(firstname, lastname)


print("Using MySQLdb…")
import MySQLdb
myConnection = MySQLdb.connect( host=hostname, user=username, passwd=password, db=database )
doQuery( myConnection )
myConnection.close()

print("Using pymysql…")
import pymysql
myConnection = pymysql.connect( host=hostname, user=username, passwd=password, db=database )
doQuery( myConnection )
myConnection.close()

print("Using mysql.connector…")
import mysql.connector
myConnection = mysql.connector.connect( host=hostname, user=username, passwd=password, db=database )
doQuery( myConnection )
myConnection.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
