---
title: mysql example LoginAndHash (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'LoginAndHash'

Functions in program: 
* `def login():`
* `def register():`
* `def check():`
* `def menu():`
* `def dbLog():`

Modules used in program: 
* `import hashlib`
* `import mysql.connector`

## python LoginAndHash

Python mysql example: LoginAndHash

```python
from Tools.scripts.treesync import raw_input
import mysql.connector
import hashlib

def dbLog():
    global mydb
    mydb = mysql.connector.connect(
        host="127.0.0.1",
        user="root",
        passwd="",
        database="yorDB"
    )

def menu():
    print('1.If you want login press "1"')
    print('2.If you want register press "2"')
    option = input()

    if option == '1':
        login()
    elif option == '2':
        register()
    else:
        print('I do not know this')
        menu()

def check():
    global password,name
    print('give data -->')
    name = raw_input('Name: ')
    password = raw_input('Password: ')
    if len(name) < 4:
        print('Name is too short')
        check()
    elif len(password) < 5:
        print('Password is too short')
        check()

def register():
    global password,name,mycursor
    check()
    result = hashlib.md5(password.encode())
    dbLog()

    mycursor = mydb.cursor()
    sql = "INSERT INTO gazdb (Name, Password) VALUES (%s, %s)" #send info for database
    val = (name, result.hexdigest())
    mycursor.execute(sql, val)
    mydb.commit()

def login():
    global mydb
    check()
    result = hashlib.md5(password.encode())
    dbLog()

    mycursor = mydb.cursor()
    sql = "SELECT * FROM gazdb WHERE Name = "+"'"+name+"'"
    mycursor.execute(sql)
    myresult = mycursor.fetchall()
    namedb = str(myresult[0][0])
    passdb = str(myresult[0][1])


    if (namedb  == name and passdb == result.hexdigest()):
        print('Nice login success')
    else:
        print('Try again')
        login()

menu()




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
