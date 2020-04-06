---
title: mysql example DB (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'DB'


Modules used in program: 
* `import mysql.connector`

## python DB

Python mysql example: DB

```python
import mysql.connector
from os import environ


class DB:

    def __init__(self):
        config = {
            'user': environ['DB_USER'],
            'password': environ['DB_PASS'],
            'host': environ['DB_HOST'],
            'ssl_ca': 'rds-combined-ca-bundle.pem',
            'database': environ['DB_SCHEMA'],
        }

        self._conn = mysql.connector.connect(**config)
        self._cur = self._conn.cursor(dictionary=True)

    def _execute(self, sql, params=None):
        """
        Executes a SQL command and returns its cursor
        "protected" method - Internal use only
        :param sql: string
        :param params: string
        :return: MySQL Connector Cursor object
        """
        if params is not None:
            cur = self._cur
            cur.execute(sql, params)
            return cur
        cur = self._cur
        cur.execute(sql)
        return cur

    def run_fa(self, sql):
        """
        Runs a SQL fetching all result lines
        :param sql: string
        :return: list of dicts or False
        """
        try:
            return self._execute(sql).fetchall()
        except Exception as e:
            print(e)
            return False

    def run_fr(self, sql):
        """
        Runs a SQL fetching one row
        :param sql: string
        :return: dict or False
        """
        try:
            return self._execute(sql).fetchone()
        except Exception as e:
            print(e)
            return False

    def run_fv(self, sql, value_name):
        """
        Runs a SQL fetching a value
        :param sql:
        :param value_name:
        :return: string or False
        """
        try:
            return self._execute(sql).fetchone()[value_name]
        except Exception as e:
            print(e)
            return False

    def run(self, sql):
        """
        Runs a SQL without result fetching
        Use with UPDATE or INSERT SQL commands
        :param sql: string
        :return: boolean
        """
        try:
            self._execute(sql)
            self._conn.commit()
            return True
        except Exception as e:
            print(e)
            return False

    def __del__(self):
        self._conn.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
