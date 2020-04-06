---
title: mysql example sql project (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'sql project'

Functions in program: 
* `def connection():`

Modules used in program: 
* `import sys`
* `import datetime`
* `import getpass`
* `import mysql.connector`

## python sql project

Python mysql example: sql project

```python
import mysql.connector
import getpass
import datetime
import sys

def connection():
    mydb = mysql.connector.connect(
        host="localhost",
        user="root",
        passwd=getpass.getpass("Password: "),
        database="Expenses"
        )
    mycursor = mydb.cursor()
# mycursor.execute("CREATE TABLE October (id INT AUTO_INCREMENT PRIMARY KEY, day VARCHAR(255), item VARCHAR(255), cost FLOAT)")

#mycursor.execute("ALTER TABLE October (id FLOAT AUTO_INCREMENT PRIMARY KEY)")

    currentDate = datetime.datetime.now().strftime("%y-%m-%d")
    sql = "INSERT INTO October (day, item, cost) VALUES (%s, %s, %s)"
    val = []
    while True:
        item = input("Please enter the name of the item: ")
        cost = float(input("Please enter the price of {}: ".format(item)))
        objects = currentDate, item, cost
        entry = val.append(objects)
        more_entries = input("Would you like to add more items?(Y/N) ")
        if more_entries == "Y".lower():
            pass
        else:
            mycursor.executemany(sql, val)
            mydb.commit()
            print(mycursor.rowcount, "records inserted with a total of {} of IDs.".format(mycursor.lastrowid))
            sys.exit("All done here...")

connection()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
