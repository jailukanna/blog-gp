---
title: mysql example streaming test (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'streaming test'


Modules used in program: 
* `import flask`
* `import mysql.connector`

## python streaming test

Python mysql example: streaming test

```python
import mysql.connector
import flask

cnx = mysql.connector.connect(user='dennis', password='Focus8Strength',
                              host='127.0.0.1',
                              database='tweetsdb')


cursor = cnx.cursor()


cursor.execute ('select word, cnt from tweets order by cnt desc limit 5')

cursor.


cursor.close()

cnx.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
