---
title: mysql example MySQL with PY (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'MySQL with PY'

Functions in program: 
* `def view():`
* `def delete():`
* `def insert():`
* `def exists(roll):`

Modules used in program: 
* `import mysql.connector`

## python MySQL with PY

Python mysql example: MySQL with PY

```python
import mysql.connector

TABLE = "student"
mydb = mysql.connector.connect(
  host="localhost",
  user="newuser",
  passwd="password",
  database="satyam"
)
mycursor = mydb.cursor()


def exists(roll):
    sql = "SELECT * FROM " + TABLE + " WHERE roll = %s"
    val = (roll,)
    mycursor.execute(sql, val)
    for x in mycursor:
        return True
    return False


def insert():
    roll_no = input("Roll No: ")
    name = input("Name: ")
    branch = input("Branch: ")
    if not exists(roll_no):
        sql = "INSERT INTO " + TABLE + " VALUES (%s, %s, %s)"
        val = (roll_no, name, branch)
        mycursor.execute(sql, val)
        print("Record inserted...")
    else:
        sql = "UPDATE " + TABLE + " SET name = %s, branch =  %s WHERE roll = %s"
        val = (name, branch, roll_no)
        mycursor.execute(sql, val)
        print("Record updated...")


def delete():
    roll_no = input("Roll No: ")
    if not exists(roll_no):
        print("Record not found...")
        return

    sql = "DELETE FROM " + TABLE + " WHERE roll = %s"
    val = (roll_no,)
    mycursor.execute(sql, val)
    print("Record deleted...")


def view():
    print("\nDisplaying records...")
    mycursor.execute("SELECT * FROM "+TABLE)
    for x in mycursor:
      print(" ".join(x))


while(True):
    print("\n----- MENU ------")
    print("1. Insert/Update record")
    print("2. View all records")
    print("3. Delete record")
    print("4. Exit")
    x = int(input("Enter your choice: "))
    if x==1:
        insert()
    elif x==2:
        view()
    elif x==3:
        delete()
    else:
        print("See you soon!")
        break
    mydb.commit()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
