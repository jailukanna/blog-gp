---
title: mysql example mysql connector sample (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql connector sample'


Modules used in program: 
* `import mysql.connector`

## python mysql connector sample

Python mysql example: mysql connector sample

```python
import mysql.connector

try:
    database_connection = mysql.connector.connect(
        host = '<host_name>',
        port = <port_number>,
        db = '<database_name>',
        user = '<database_user>',
        passwd = '<database_password>',
        charset = '<database_character_set>',
    )

    # cursor の引数にdictionary = True を指定すると、結果セットが辞書（ハッシュ）になる
    current_cursor = database_connection.cursor(dictionary = True)

    current_cursor.execute("SELECT * FROM <table_name>;")
    records = current_cursor.fetchall()
    for record in records:
      for key, value in record.items():
        print("%s: %s" % (key, value))
      print("---")
        
except Exception as error:
    database_connection.rollback()
    raise error

finally:
    current_cursor.close
    database_connection.close

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
