---
title: mysql example android chat msg notification 2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'android chat msg notification 2'


Modules used in program: 
* `import sys`
* `import mysql.connector`

## python android chat msg notification 2

Python mysql example: android chat msg notification 2

```python
import mysql.connector
import sys

try:
    chat_db = mysql.connector.connect(host="localhost", user="root", passwd="ahmedgad", database="chat_db")
except:
    sys.exit("Error connecting to the database. Please check your inputs.")

db_cursor = chat_db.cursor()

add_column_query = "ALTER TABLE users ADD last_date_conversations_fetched TIMESTAMP DEFAULT CURRENT_TIMESTAMP"
db_cursor.execute(add_column_query)
chat_db.commit()

add_column_query = "ALTER TABLE users ADD notified_by_new_messages BOOLEAN DEFAULT false"
db_cursor.execute(add_column_query)
chat_db.commit()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
