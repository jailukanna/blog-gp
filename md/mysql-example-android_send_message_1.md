---
title: mysql example android send message 1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'android send message 1'


Modules used in program: 
* `import sys`
* `import mysql.connector`

## python android send message 1

Python mysql example: android send message 1

```python
import mysql.connector
import sys

try:
    chat_db = mysql.connector.connect(host="localhost", user="root", passwd="ahmedgad", database="chat_db")
except:
    sys.exit("Error connecting to the database. Please check your inputs.")

db_cursor = chat_db.cursor()

try:
    db_cursor.execute("CREATE TABLE messages (id INT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY, sender_username VARCHAR(255) NOT NULL, receiver_username VARCHAR(255) NOT NULL, message TEXT NOT NULL, receive_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP, FOREIGN KEY (receiver_username) REFERENCES users(username), FOREIGN KEY (sender_username) REFERENCES users(username))")
    print("messages table created successfully.")
except mysql.connector.DatabaseError:
    sys.exit("Error creating the table. Please check if it already exists.")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
