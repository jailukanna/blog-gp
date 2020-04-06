---
title: mysql example import csv (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'import csv'


Modules used in program: 
* `import datetime`
* `import mysql.connector`
* `import codecs`
* `import urllib.request`
* `import csv`

## python import csv

Python mysql example: import csv

```python
import csv
import urllib.request
import codecs
import mysql.connector
import datetime

url = input("Enter the CSV's URL: ")
webpage = urllib.request.urlopen(url)
datareader = csv.reader(codecs.iterdecode(webpage, 'utf-8'))

date_time = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")

countries_list = []
for row in datareader:
    countries_list.append(row[0])  # Add all countries to the defined list

countries = list(set(countries_list))  # Get unique values from the list
for country in countries:
    if country != '':  # Check if there's an empty column in the csv
        countries.append((country, date_time, date_time))

connection = mysql.connector.connect(host='localhost', database='db', user='user', password='password')
try:
    country_insert_query = """ INSERT INTO countries (name, created_at, updated_at) VALUES (%s,%s,%s) """
    cursor = connection.cursor(prepared=True)
    result = cursor.executemany(country_insert_query, countries)
    connection.commit()
    print(cursor.rowcount, "Record inserted successfully into countries table")

except mysql.connector.Error as error:
    print(f"Failed inserting record into python_users table { error }")

finally:
    # closing database connection.
    if connection.is_connected():
        connection.close()
        print("Connection closed")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
