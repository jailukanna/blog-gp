---
title: mysql example mysql 4 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql 4'


Modules used in program: 
* `import mysql.connector`

## python mysql 4

Python mysql example: mysql 4

```python
import mysql.connector

banco = mysql.connector.connect(
    host="localhost",
    user="root",
    passwd="",
    database="python_youtube"
)

cursor = banco.cursor()

comando_sql = "SELECT * FROM pessoas"
cursor.execute(comando_sql)

dados_lidos = cursor.fetchall()

print(dados_lidos)

abcd

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
