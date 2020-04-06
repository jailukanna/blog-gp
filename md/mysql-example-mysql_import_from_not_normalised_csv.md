---
title: mysql example mysql import from not normalised csv (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql import from not normalised csv'

Functions in program: 
* `def csvLinesSkipFirst(csvPath):`

Modules used in program: 
* `import sys`
* `import mysql.connector`
* `import csv`

## python mysql import from not normalised csv

Python mysql example: mysql import from not normalised csv

```python
#!/usr/local/bin/python
import csv
import mysql.connector
import sys

def csvLinesSkipFirst(csvPath):
    csv_data = csv.reader(file(csvPath))
    next(csv_data)
    return csv_data

db = mysql.connector.connect(
    host='***',
    user='***',
    passwd='***',
    db='***'
)
cursor = db.cursor()

# conditional truncate
if '--truncate' in sys.argv:
    truncate = True
else:
    truncate = 'y' == raw_input("Truncate tables before inserting ? [y/n]").lower()

if truncate:
    cursor.execute("SET FOREIGN_KEY_CHECKS = 0; ")
    cursor.execute("truncate table orders; ")
    cursor.execute("truncate table events; ")
    cursor.execute("truncate table customers; ")
    cursor.execute("truncate table offers; ")
    cursor.execute("SET FOREIGN_KEY_CHECKS = 1;")
    db.commit()
    print('Tables truncated')

# Customers
customers_name_to_id = {}
for row in csvLinesSkipFirst('data/customers.csv'):
    cursor.execute('INSERT INTO customers (name, credit_balance) VALUES(%s, %s)', row)
    customers_name_to_id[row[0].strip().lower()] = cursor.lastrowid

# Offers
offer_name_to_id = {}
for row in csvLinesSkipFirst('data/offers.csv'):
    cursor.execute('INSERT INTO offers (name, price) VALUES(%s, %s)', row)
    offer_name_to_id[row[0].strip().lower()] = cursor.lastrowid

# events
events_name_to_id = {}
for row in csvLinesSkipFirst('data/orders.csv'):
    current_event_name = row[4]
    if current_event_name.strip().lower() not in events_name_to_id:
        cursor.execute('INSERT INTO events (event_name) VALUES(%s)', [current_event_name])
        events_name_to_id[current_event_name.strip().lower()] = cursor.lastrowid

print("Customers: " + str(customers_name_to_id))
print("Offers: " + str(offer_name_to_id))
print("Events: " + str(events_name_to_id))

# orders
for row in csvLinesSkipFirst('data/orders.csv'):
    try:
        customer_id = int(customers_name_to_id[row[0].strip().lower()])
        offer_id = int(offer_name_to_id[row[2].strip().lower()])
        event_id = int(events_name_to_id[row[4].strip().lower()])
        insertRow = (customer_id, offer_id, int(row[3]), event_id)
        print((insertRow))
        cursor.execute(
            "INSERT INTO orders "
            "(customer_id, offer_id, quantity, event_id) "
            "VALUES"
            "(%s, %s, %s, %s)",
            insertRow
        )
        db.commit()
        # sys.exit()
    except KeyError as e:
        print("Order add skipped. missing key. " + repr(e))
    except Exception as e:
        print("Order add skipped. Details:" + repr(e))
        sys.exit()


db.commit()
cursor.close()

print("Done")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
