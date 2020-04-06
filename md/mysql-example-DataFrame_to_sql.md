---
title: mysql example DataFrame to sql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'DataFrame to sql'


Modules used in program: 
* `import mysql.connector`

## python DataFrame to sql

Python mysql example: DataFrame to sql

```python
import mysql.connector
mydb = mysql.connector.connect(
    host='localhost',
    user='root',
    password='****',
    database='football_db'
)
print(mydb)
print(players_data.columns)

cursor = mydb.cursor()

query = '''CREATE TABLE player_data
( Name text(255), Age int(10), Nationality text(255), Overall int(10), Potential text(255), Club text(255),
        Value_Millions int(10)… 

);'''

cursor.execute(query)

players_data.to_sql(name='player_data', con=engine, index=False, if_exists='append')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
