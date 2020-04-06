---
title: mysql example python sql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'python sql'

Functions in program: 
* `def update_data():`
* `def order_by():`
* `def select_one_item():`
* `def select_all_data():`
* `def insert_data():`
* `def show_table():`
* `def create_table():`
* `def create_database():`

Modules used in program: 
* `import pymysql`
* `import mysql.connector`

## python python sql

Python mysql example: python sql

```python
import mysql.connector


mydb = mysql.connector.connect(
  host="127.0.0.1",
  user="root",
  passwd="XXXXXX"
)


print(mydb)
mycursor = mydb.cursor()


def create_database():
	mycursor = mydb.cursor()
	mycursor.execute("CREATE DATABASE mydatabase")


def create_table():
	mycursor.execute("CREATE TABLE customers (name VARCHAR(255), address VARCHAR(255))")


def show_table():
	mycursor.execute("SHOW TABLES")
	for x in mycursor:
	     print(x)


def insert_data():
	sql = "INSERT INTO customers (name, address) VALUES (%s, %s)"
	val = ("John", "Highway 21")
	mycursor.execute(sql, val)
	mydb.commit()
	print(mycursor.rowcount, "record inserted.")


def select_all_data():
	mycursor.execute("SELECT name, address FROM customers")
	myresult = mycursor.fetchall()
	for x in myresult:
	  print(x)


def select_one_item():
	mycursor.execute("SELECT name, address FROM customers")
	myresult = mycursor.fetchone()
	print(myresult)


def order_by():
	sql = "SELECT * FROM customers ORDER BY name"
	mycursor.execute(sql)
	myresult = mycursor.fetchall()
	for x in myresult:
	  print(x)


def update_data():
	sql = "UPDATE customers SET address = 'Canyon 123' WHERE address = 'Valley 345'"
	mycursor.execute(sql)
	mydb.commit()
	print(mycursor.rowcount, "record(s) affected")



create_database()




import pymysql
conn = pymysql.connect(host='127.0.0.1',port = 3306,  unix_socket='/var/run/mysqld/mysqld.sock', user='codephillip', passwd="xxxxxxx", db='mysql')
cur = conn.cursor()
cur.execute("SELECT Host,User FROM user")
for response in cur:
    print(response)
cur.close()
conn.close()



# CREATE TABLE
CREATE TABLE Persons (
    PersonID int,
    LastName varchar(255),
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255) 
);


CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);


# ALTER
ALTER TABLE Persons
ADD DateOfBirth date;



# UNIQUE
CREATE TABLE Persons (
    ID int NOT NULL UNIQUE,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int
);


# ID AUTO INCREMENT
CREATE TABLE Persons (
    ID int NOT NULL AUTO_INCREMENT,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    PRIMARY KEY (ID)
);

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
