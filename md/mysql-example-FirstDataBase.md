---
title: mysql example FirstDataBase (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'FirstDataBase'

Functions in program: 
* `def menu():`
* `def seeDb():`
* `def calculate():`
* `def statistics():`

Modules used in program: 
* `import mysql.connector`

## python FirstDataBase

Python mysql example: FirstDataBase

```python
from datetime import datetime
from datetime import date
import mysql.connector
from pip._vendor.distlib.compat import raw_input

def statistics():
    global date
    date = str(date.today())  #variable accepts date
    value = raw_input('Podaj aktualny stan licznika: ')
    value = int(value)   #take value for unser
    now = datetime.now()  #take timestamp
    timeSNow = round(datetime.timestamp(now)) #cut dot
    timeSNow = int(timeSNow) #conwert to int

    mydb = mysql.connector.connect(
        host="127.0.0.1",
        user="root",
        passwd="",
        database="nameDataBase"
        )

    mycursor = mydb.cursor()
    mycursor.execute("SELECT * FROM gazdb ORDER BY id DESC LIMIT 1") #take record from data base
    myresult = mycursor.fetchone()
    valueV2 = myresult[1]  #assign to variable index number 1
    valueV2 = int(valueV2)
    timeSNowV2 = myresult[2] #assign to variable index number 2
    timeSNowV2 = int(timeSNowV2)
    lapsing = round((timeSNow - timeSNowV2)/86400)
    used = value-valueV2
    cost = round(used * 2.31)

    mycursor = mydb.cursor()
    sql = "INSERT INTO gazdb (date, value, time) VALUES (%s, %s, %s)" #send info for database
    val = (date, value, timeSNow)
    mycursor.execute(sql, val)
    mydb.commit()

    used = str(used)
    lapsing = str(lapsing)
    cost = str(cost)
    print('Przez ostatnie |'+lapsing+'| dni (liczone od '+date+'), zostalo zuzyte |'+used+'| kubikow gazu.(Co kosztowalo nas '
          +cost+' złotych)')

def calculate():
    m3 = raw_input('Podaj ilosc kubikow: ')
    m3 = int(m3)
    valueM3 = m3 * 2.31
    valueM3 = str(valueM3)
    m3 = str(m3)
    print(m3+' kubikow bedize kosztowac '+valueM3+' zlotych')

def seeDb():

    mydb = mysql.connector.connect(
        host="127.0.0.1",
        user="root",
        passwd="",
        database="nameDataBase"
    )
    mycursor = mydb.cursor()

    mycursor.execute("SELECT * FROM gazdb") #take e raport of dataBase

    myresult = mycursor.fetchall()
     
    for x in myresult:
        print(x)

def menu():
    print('1.Jezeli chcesz obliczyc zuzycie kliknij "1"')
    print('2.Jezeli chcesz sprawdzic cene kubikow kliknij "2"')
    print('3.Jezeli chcesz wyswietlic ostatnie 100 informacji z bazy kliknij "3"')
    butt = input()
    butt = int(butt)
    print(butt)
    if butt == 1:
        statistics()
    elif butt == 2:
        calculate()
    elif butt == 3:
        seeDb()
    else:
        print('nie znam tego polecenia')


menu()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
