---
title: mysql example mysqlsample1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysqlsample1'


Modules used in program: 
* `import mysql.connector`

## python mysqlsample1

Python mysql example: mysqlsample1

```python
# -*- coding: utf-8 -*-

import mysql.connector

# データベース接続
connection = mysql.connector.connect(
            user     = 'root',
            password = 'passwd',
            host     = 'localhost',
            database = 'TestDB',
            charset  = 'utf8')
# カーソル取得
cursor = connection.cursor()
# 検索条件
conditions = {
        'ID' : '1234' # IDが1234のレコード
    }
# クエリ実行
cursor.execute('SELECT ID, TITLE FROM TEST_TBL WHERE ID = %(ID)s',
               conditions)
# 検索結果取得
records = cursor.fetchall()

# 検索結果出力
for record in records:
    for field in record:
        print(field)

# 後処理
cursor.close()
connection.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
