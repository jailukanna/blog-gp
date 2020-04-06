---
title: mysql example rds rotate general log (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'rds rotate general log'


Modules used in program: 
* `import mysql.connector`

## python rds rotate general log

Python mysql example: rds rotate general log

```python
#!/usr/bin/env python

#
# Modules Import
#
import mysql.connector
from mysql.connector import errorcode

#
# Variables Definition
#
db_config = {
    'user': '<user>',
    'password': '<password>',
    'host': '<rds_endpoint>',
    'database': 'mysql',
    'raise_on_warnings': True,
    'time_zone': 'UTC',
}
procedure = 'rds_rotate_general_log'

#
# Main
#

# Database connection with error handling
try:
    cnx = mysql.connector.connect(**db_config)
except mysql.connector.Error as err:
    if (err.errno == errorcode.ER_ACCESS_DENIED_ERROR):
        print('Something is wrong with your user name or password')
    elif (err.errno == errorcode.ER_BAD_DB_ERROR):
        print('Database does not exist')
    else:
        print(err)
else:
    # Create cursor for query execution
    cursor = cnx.cursor()

    # Execute stored procedure that rotates general_log table
    cursor.callproc('rds_rotate_general_log')

    # Print execution result
    # for result in cursor.stored_results():
    #     print(result.fecthall())

    # Close database connection
    cnx.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
