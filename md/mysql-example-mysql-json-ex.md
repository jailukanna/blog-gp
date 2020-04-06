---
title: mysql example mysql-json-ex (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql-json-ex'


Modules used in program: 
* `import json`
* `import mysql.connector`

## python mysql-json-ex

Python mysql example: mysql-json-ex

```python
import mysql.connector
import json

config = {
    "user":     "",
    "password": "",
    "host":     "localhost",
    "database": "test",
    "use_pure": True,
}

json_data = {
    "Name":         "Harry Potter",
    "Email":        "harry@earth.world",
    "CellPhone":    "xxx0000xxx",
    "Address":      "Earth, Milky Way!!"
}

b_data = json.dumps(json_data)
docID = 1

cnx = mysql.connector.connect(**config)
cursor = cnx.cursor(prepared=True)

# stmt = "UPDATE JSONDoc SET Data = JSON_REPLACE(Data, CONCAT('$', %s), CAST(%s AS JSON)) WHERE DocID=%s"
stmt = "UPDATE JSONDoc SET Data = JSON_REPLACE(Data, CONCAT('$', %s), %s) WHERE DocID=%s"
cursor.execute(stmt, ("", b_data, docID))
cnx.commit()
cursor.close()
cnx.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
