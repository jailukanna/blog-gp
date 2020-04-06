---
title: mysql example connect mysqldb (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'connect mysqldb'

Functions in program: 
* `def get_config():`

Modules used in program: 
* `import mysql.connector`
* `import configparser`

## python connect mysqldb

Python mysql example: connect mysqldb

```python
# -*- coding: utf-8 -*-
"""
Purpose: Connect to mysql database using config file.
"""

from __future__ import unicode_literals
from __future__ import print_function

import configparser
import mysql.connector

class DBConnection(object):
    """
    Connect to mysql using config.ini file.
    """
    config_file = 'config.ini'

    def __init__(self, get_config_parser, env):
        self.env = env
        self.get_config_parser = get_config_parser
        self.host = self.get_config_parser[self.env]['host']
        self.user = self.get_config_parser[self.env]['user']
        self.password = self.get_config_parser[self.env]['password']
        self.database = self.get_config_parser[self.env]['database']

    def show_data(self):
        """
        Connect to db using credentails.
        """
        try:
            conn = mysql.connector.connect(
                host=self.host, user=self.user, password=self.password, database=self.database)
            if conn.is_connected():
                print("Connected to db")
                cur = conn.cursor() # cursor object for executing SQL statements
                cur.execute("select * from exam")
                result = cur.fetchall()
                print(result)

            else:
                print("Connection failed")

        except mysql.connector.Error:
            print("Error in connection")

        finally:
            conn.close()
            print("Connection close")


def get_config():
    """
    Read file and parse
    """
    db_config = configparser.ConfigParser()
    db_config.read(DBConnection.config_file)
    return db_config

if __name__ == '__main__':
    DB = DBConnection(get_config(), 'dev')
    DB.show_data()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
