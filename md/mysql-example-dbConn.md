---
title: mysql example dbConn (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'dbConn'

Functions in program: 
* `def closedbConn(dbConn):`
* `def getdbConn()`

Modules used in program: 
* `import mysql.connector`
* `import ConfigParser`

## python dbConn

Python mysql example: dbConn

```python
import ConfigParser
import mysql.connector


def getdbConn()
  config = ConfigParser.RawConfigParser()
  config.read('sampleDb.properties')
  user = config.get('Database', 'user')
  pwd = config.get('Database', 'pwd')
  host = config.get('Database', 'host')
  dbName = config.get('Datbase', 'dbname')
  dbConn = mysql.connector.connect(user=user, password=pwd, host=host, database=dbname)
  return dbConn

def closedbConn(dbConn):
  dbConn.close()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
