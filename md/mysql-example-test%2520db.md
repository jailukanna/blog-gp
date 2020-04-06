---
title: mysql example test%2520db (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'test%2520db'


Modules used in program: 
* `import mysql.connector`

## python test%2520db

Python mysql example: test%2520db

```python
import mysql.connector

mydb = mysql.connector.connect(
    host="localhost",
    user="root",
    passwd="your password on sql",
    database="basicdatabase"
)
mycursor = mydb.cursor()

mycursor.execute("CREATE DATABASE basicdatabase")

mycursor.execute("CREATE TABLE Clients (id INTEGER(10), address VARCHAR(225), name VARCHAR(225), age INTEGER(3))")

sqlFormula = "INSERT INTO Clients (id, address, name, age) VALUES (%s, %s, %s, %s)"
person = [(323409504, "2784 Burwell Heights Road", "Zack", 22),
          (342121112, "4714  Baker Street", "Manon French", 23),
          (989737493, "4246  Brew Creek Rd", "Landon Mcdonald", 42),
          (123723843, "865  St Jean Baptiste St", "Ella-Grace Tang", 56),
          (232178234, "825  th Ave", "Maleeha Leblanc", 27),
          (233434244, "4354  James Street", "Garfield Weber", 29),
          (231236784, "165  Whitmore Road", "Arjan Crane", 97),
          (111100303, "3139  St. John Street", "Owen Lynch", 55),
          (342848384, "3106  St. John Street", "Dominick Landry", 78),
          (455585839, "3377  Fallon Drive", "Joseff Devlin", 45),
          (342342211, "3376  Fallon Drive", "Rizwan Dodd", 67),
          (934049348, "3114  Waterton Avenue", "Jules Henderson", 25),
          (888888374, "3128  Waterton Avenue", "Mcauley Meza", 23),
          (122237843, "3896  Isabella Street", "Samual Laing", 23),
          (776654534, "2341  Eglinton Avenue", "Waqar Estes", 65),
          (138934344, "5005  avenue Royale", "Thea Bell", 87),]

mycursor.executemany(sqlFormula, person)

mydb.commit()































```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
