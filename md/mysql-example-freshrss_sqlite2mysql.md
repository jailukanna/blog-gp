---
title: mysql example freshrss sqlite2mysql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'freshrss sqlite2mysql'


Modules used in program: 
* `import sqlite3`
* `import mysql.connector                                              `

## python freshrss sqlite2mysql

Python mysql example: freshrss sqlite2mysql

```python
#!/usr/bin/env python   
import mysql.connector                                              
import sqlite3
                                                                    
#                                                                   
# Configuration
#                                                                   
                                  
freshrss_username = "killruana"
sqlite_db = "db.sqlite"
mysql_host = "localhost"
mysql_username = "freshrss"
mysql_password = ""      
mysql_database = "freshrss"

tables = ["category", "feed", "entry", "tag", "entrytag"]

#
# Connect to the databases
#
mysql_connection = mysql.connector.connect(
    host=mysql_host,
    user=mysql_username,
    password=mysql_password,
    database=mysql_database,
)
sqlite_connection = sqlite3.connect(sqlite_db)
sqlite_connection.text_factory = (
    bytes  # Avoid sqlite3.OperationalError when trying to fetch raw data
)

#
# Clean up the MySQL database
#

print("Cleaning up the MySQL database...")
for table in reversed(tables):
    print(" -", table)
    mysql_cursor = mysql_connection.cursor()
    mysql_cursor.execute("DELETE FROM {}_{}".format(freshrss_username, table))
    mysql_cursor.execute(
        "ALTER TABLE {}_{} AUTO_INCREMENT = 1".format(freshrss_username, table)
    )
    mysql_connection.commit()

#
# Migrate the data
#

print("Importing tables...")
for table in tables:
    print(" -", table)

    mysql_cursor = mysql_connection.cursor()
    mysql_cursor.execute("DESCRIBE {}_{}".format(freshrss_username, table))
    columns = [row[0] for row in mysql_cursor.fetchall()]

    insert_query = "INSERT INTO {}_{} ({}) VALUES ({})".format(
        freshrss_username,
        table,
        ", ".join(columns),
        ", ".join("%s" for _ in range(len(columns))),
    )

    # Schema hacks
    if table == "entry":
        columns[columns.index("content_bin")] = "content"

    for row in sqlite_connection.execute(
        "SELECT {} FROM {}".format(", ".join(columns), table)
    ):
        mysql_cursor.execute(insert_query, row)
    mysql_connection.commit()

#
# Close connections
#
sqlite_connection.close()
mysql_connection.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
