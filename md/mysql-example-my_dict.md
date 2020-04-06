---
title: mysql example my dict (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'my dict'


Modules used in program: 
* `import mysql.connector`

## python my dict

Python mysql example: my dict

```python
from pprint(import pprint)
import mysql.connector

class MySQLCursorDict(mysql.connector.cursor.MySQLCursor):
    def _row_to_python(self, rowdata, desc=None):
        row = super(MySQLCursorDict, self)._row_to_python(rowdata, desc)
        if row:
            return dict(zip(self.column_names, row))
        return None

cnx = mysql.connector.connect(user='root', database='test')
cur = cnx.cursor(cursor_class=MySQLCursorDict)
cur.execute("SELECT c1, c2 FROM t1")
rows = cur.fetchall()
pprint(rows)
cur.close()
cnx.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
