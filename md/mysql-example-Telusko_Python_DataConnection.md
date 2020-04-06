---
title: mysql example Telusko Python DataConnection (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Telusko Python DataConnection'


Modules used in program: 
* `import mysql.connector as mc`

## python Telusko Python DataConnection

Python mysql example: Telusko Python DataConnection

```python
import mysql.connector as mc

conn = mc.connect(host="localhost",user="mukund",password="mukund#7",database="telusko")

cursr = conn.cursor()

cursr.execute("select * from student")

result = cursr.fetchall()
#result = cursr.fetchall()



for i in result:
    print(i)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
