---
title: mysql example mysql-test (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql-test'


Modules used in program: 
* `import mysql.connector`

## python mysql-test

Python mysql example: mysql-test

```python
import mysql.connector

cnx = mysql.connector.connect(user='user', password='pass',
                              host='127.0.0.1')
                  
				  
				  
cursor = cnx.cursor()

query = "SELECT company_name, company_mail, company_state FROM scrapped_mails"

cursor.execute(query)

for (company_name, company_mail, company_state) in cursor:
  print("{}, {} - {}")

cursor.close()
cnx.close()
print("Done with myseldibs.py")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
