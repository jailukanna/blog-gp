---
title: mysql example test mysql connector (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'test mysql connector'


Modules used in program: 
* `import mysql.connector`

## python test mysql connector

Python mysql example: test mysql connector

```python
# 06/2019
# test mysql connector
# install with: pip3 install mysql-connector

import mysql.connector

mydb = mysql.connector.connect(
    host="localhost",
    user="root",
    passwd="11111",
    database="my_test_database"
)

print(mydb)

mycursor = mydb.cursor()


# ---- drop table

sql = "DROP TABLE IF EXISTS customers"
mycursor.execute(sql)


# ---- create table
mycursor.execute("CREATE TABLE `customers` (`name` VARCHAR(255), `address` VARCHAR(255))")


# ---- insert one

sql = "INSERT INTO customers (name, address) VALUES (%s, %s)"
val = ("John", "Highway 21")
mycursor.execute(sql, val)
mydb.commit()
print("1 record inserted, ID:", mycursor.lastrowid)


# ---- insert many

sql = "INSERT INTO customers (name, address) VALUES (%s, %s)"
val = [
    ('Peter', 'Lowstreet 4'),
    ('Amy', 'Apple st 652'),
    ('Hannah', 'Mountain 21'),
    ('Michael', 'Valley 345'),
    ('Sandy', 'Ocean blvd 2'),
    ('Betty', 'Green Grass 1'),
    ('Richard', 'Sky st 331'),
    ('Susan', 'One way 98'),
    ('Vicky', 'Yellow Garden 2'),
    ('Ben', 'Park Lane 38'),
    ('William', 'Central st 954'),
    ('Chuck', 'Main Road 989'),
    ('Viola', 'Sideway 1633')
]
mycursor.executemany(sql, val)
mydb.commit()
print(mycursor.rowcount, "was inserted.")


# ---- fetch many

mycursor.execute("SELECT * FROM customers")
myresult = mycursor.fetchall()
for x in myresult:
    print(x)


# ---- fetch one

mycursor.execute("SELECT * FROM customers LIMIT 1")
myresult = mycursor.fetchone()
print("one fetched: ", myresult)


# ---- delete

sql = "DELETE FROM customers WHERE address = %s"
adr = ("Yellow Garden 2", )
mycursor.execute(sql, adr)
mydb.commit()
print(mycursor.rowcount, "record(s) deleted")



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
