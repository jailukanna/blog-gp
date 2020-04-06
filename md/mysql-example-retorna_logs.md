---
title: mysql example retorna logs (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'retorna logs'


Modules used in program: 
* `import config.mysql as dbconfig`

## python retorna logs

Python mysql example: retorna logs

```python
import config.mysql as dbconfig
from MysqlFactory import MysqlFactory

connection = MysqlFactory().getConnection(dbconfig.params)
cursor = connection.cursor()

query = """
		SELECT 
			*
		FROM
			log
		WHERE
			log.id_course is null
"""

cursor.execute(query)

result_set = cursor.fetchall();

print(cursor.rowcount)
connection.close();

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
