---
title: mysql example mysql test connection (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql test connection'

Functions in program: 
* `def main():`
* `def write_line(out):`

Modules used in program: 
* `import time`
* `import logging`
* `import sys`
* `import mysql.connector`

## python mysql test connection

Python mysql example: mysql test connection

```python
#!/usr/bin/env python
""" mysql_test_connection.py: simple script to check if mysql drops connections
"""
import mysql.connector
from mysql.connector import errorcode
import sys
import logging
import time

TIMEOUT = 100.0 / 1000  # milliseconds
CONFIG = {
    'user': 'root',
    'password': '',
    'host': '127.0.0.1',
    'database': 'mysql',
}


class Tester(object):
    def __init__(self):
        self.cnx = None
        self.counter_connections = 0
        self.counter_errors = 0
        self.counter_queries = 0
        self.cursor = None
        logging.basicConfig(filename='mysql.log', level=logging.INFO)
        logging.info('Started')

    def stats(self):
        return "connections: {c}, queries:{q} , errors: {e}".format(
                c=self.counter_connections,
                e=self.counter_errors,
                q=self.counter_queries,
        )

    def connect(self):
        try:
            self.cnx = mysql.connector.connect(**CONFIG)
            self.cursor = self.cnx.cursor()

        except mysql.connector.Error as err:
            logging.error(err)
            self.counter_errors += 1
        else:
            self.counter_connections += 1

    def execute(self, query):
        try:
            self.cursor = self.cnx.cursor(buffered=True)
            self.cursor.execute(query)
        except mysql.connector.Error as err:
            logging.error(err)
            self.counter_errors += 1
        else:
            self.counter_queries += 1

        return self.cursor

    def loop(self):
        while True:
            self.connect()
            self.execute("SHOW FULL PROCESSLIST")
            self.cnx.close()
            self.cnx = None
            self.cursor = None
            write_line(self.stats())
            time.sleep(TIMEOUT)

    def __del__(self):
        if self.cnx is not None:
            self.cnx.close()
        logging.info(self.stats())
        logging.info('Finished')


def write_line(out):
    sys.stdout.write("\r{}".format(out))
    sys.stdout.flush()


def main():
    tester = Tester()
    try:
        tester.loop()
    except (KeyboardInterrupt, SystemExit):
        print("\nProcess finished")
        sys.exit(0)


if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
