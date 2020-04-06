---
title: mysql example sample db toire (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'sample db toire'


Modules used in program: 
* `import mysql.connector `

## python sample db toire

Python mysql example: sample db toire

```python
dbconfig = {'host': '127.0.0.1',
             'user': 'toire',
             'password': 'toire',
             'database': 'sampleDB',}
import mysql.connector 
conn = mysql.connector.connect(**dbconfig)
cursor = conn.cursor()

# テーブル名を表示する
_SQL = """show tables"""
cursor.execute(_SQL)
res = cursor.fetchall()
# >>>ress  => [('log',)]

# テーブルを表示する
_SQL = """describe log"""
cursor.execute(_SQL)
res = cursor.fetchall()
for row in res:
     print(row)
# ('id', 'int(11)', 'NO', 'PRI', None, 'auto_increment')
# ('ts', 'timestamp', 'NO', '', 'CURRENT_TIMESTAMP', '')
# ('phrase', 'varchar(128)', 'NO', '', None, '')
# ('letters', 'varchar(32)', 'NO', '', None, '')
# ('ip', 'varchar(16)', 'NO', '', None, '')
# ('browser_string', 'varchar(256)', 'NO', '', None, '')
# ('results', 'varchar(64)', 'NO', '', None, '')



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
