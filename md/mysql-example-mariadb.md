---
title: mysql example mariadb (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mariadb'


Modules used in program: 
* `import json`
* `import mysql.connector as mariadb`

## python mariadb

Python mysql example: mariadb

```python
"""
To connect to mariadb you would need to conda install the mysql-connector-python package

conda install -c anaconda mysql-connector-python
"""
import mysql.connector as mariadb
import json


# Read the credentials from secret
credentials = None
with open('/var/run/secrets/user_credentials/mariadb_credentials') as f:
    credentials = json.load(f)

# Ensure your credentials were setup
if credentials:
    # Connect to the DB
    connection = mariadb.connect(
        user=credentials.get('user'),
        password=credentials.get('password'),
        database=credentials.get('database'),
        host=credentials.get('host')
    )
    cursor = connection.cursor()

    # Execute the query
    cursor.execute("SELECT first_name, last_name FROM employees LIMIT 20")

    # Loop through the results
    for first_name, last_name in cursor:
        print(f'First name: {first_name}, Last name: {last_name}')
        
    # Close the connection
    connection.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
