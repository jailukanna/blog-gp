---
title: PIL example Image01 img db (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'Image01 img db'

Functions in program: 
* `def read_tbl(conn, table):`
* `def open_db():`

Modules used in program: 
* `import sqlite3 as db`

## python Image01 img db

Python PIL example: Image01 img db

```python
import sqlite3 as db

def open_db():
    try:
        conn = db.connect('imgstat.db',)
        print('Open image database')
        return conn

    except db.Error as e:

        print('DB connection error: ', e)

    return None


def read_tbl(conn, table):
    sql = 'select a. exifvalue, a.cnt from (select exiftag, exifvalue, count(exiftag) as cnt from picdata where exiftag = "EXIF FocalLength" group by exiftag,exifvalue) as a'
    cur = conn.cursor()
    cur.execute(sql)

    rows = cur.fetchall()

    return rows



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
