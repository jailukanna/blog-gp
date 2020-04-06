---
title: mysql example DatabaseTest (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'DatabaseTest'

Functions in program: 
* `def startTest():`
* `def loadInDatabase(tableNumber, recordsToInsert, insertInBlock):`
* `def singleValueSet(tableNumber):`
* `def singleBlockQuery(tableNumber):`
* `def singleQuery(tableNumber):`

Modules used in program: 
* `import threading`
* `import time`
* `import sys`
* `import random`
* `import mysql.connector`

## python DatabaseTest

Python mysql example: DatabaseTest

```python
import mysql.connector
import random
import sys
import time
import threading


DB_host="localhost"
DB_port="3307"
DB_user="root"
DB_passwd="toor"
DB_database="test"

multiThreading = True
threadNum = 3

# Default values, can be changed with arguments
tableNumber = 5
recordsToInsert = 1000000
insertInBlock = True
blockSize = 500


def singleQuery(tableNumber):
    sql = "INSERT INTO table" + str(tableNumber) + " VALUES "
    sql += singleValueSet(tableNumber)
    sql += ";"
    return sql


def singleBlockQuery(tableNumber):
    global blockSize

    sql = "INSERT INTO table" + str(tableNumber) + " VALUES "
    for i in range (1, blockSize + 1):
        sql += singleValueSet(tableNumber)
        if i < blockSize:
            sql += ", "
    sql += ";"
    return sql


def singleValueSet(tableNumber):
    valueSet = " ("
    for i in range (1, tableNumber + 1):
        valueSet = valueSet + str(round(random.random(), 5))
        if (i != tableNumber):
            valueSet += ", "
    valueSet += ")"
    return valueSet


def loadInDatabase(tableNumber, recordsToInsert, insertInBlock):

    mydb = mysql.connector.connect(
        host=DB_host,
        port=DB_port,
        user=DB_user,
        passwd=DB_passwd,
        database=DB_database
    )

    mycursor = mydb.cursor()

    insertedRecords = 0
    while insertedRecords < recordsToInsert:
        if insertInBlock:
            sql = singleBlockQuery(tableNumber)
            mycursor.execute(singleBlockQuery(tableNumber), multi=True)
            insertedRecords += mycursor.rowcount
        else:
            mycursor.execute(singleQuery(tableNumber))
            insertedRecords += mycursor.rowcount

        mydb.commit()    


def startTest():
    global tableNumber
    global recordsToInsert

    start = time.time()

    threads = []
    
    if len(sys.argv) > 2:
        tableNumber = int(sys.argv[1])
        recordsToInsert = int(sys.argv[2])

    if multiThreading:
        for num in range(0, threadNum):
            thread = threading.Thread(target = loadInDatabase, args = (tableNumber, recordsToInsert / threadNum, insertInBlock))
            thread.start()
            threads.append(thread)
        for thread in threads:
            thread.join()
    else:
        loadInDatabase(tableNumber, recordsToInsert, insertInBlock)

    end = time.time()
    delta = end - start

    print("Took", delta ,"seconds to insert ", recordsToInsert, " records")
    print("Inserts per second: ", recordsToInsert/delta)


if __name__ == "__main__":
    startTest()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
