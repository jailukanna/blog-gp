---
title: mysql example cyn sql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'cyn sql'


Modules used in program: 
* `import logging`
* `import mysql.connector`

## python cyn sql

Python mysql example: cyn sql

```python
#!/usr/bin/python3

import mysql.connector
import logging


class ql_sql:
    def __init__(self, conf):
        self.conf = conf
        self.log  = logging.getLogger('ftth-mon.' + __name__)
        self.log.debug('Initializing sql')


    def connect(self):
        try:
            self.cnx = mysql.connector.connect(
                user     = self.conf['user'],
                password = self.conf['pass'],
                host     = self.conf['host'],
                database = self.conf['base']
            )
        except mysql.connector.Error as err:
            if err.errno == mysql.connector.errorcode.ER_ACCESS_DENIED_ERROR:
                self.log.error("MySQL: Access denied");
            elif err.errno == mysql.connector.errorcode.ER_BAD_DB_ERROR:
                self.log.error("MySQL: Database does not exist")
            else:
                self.log.error("MySQL: {}".format(err))

            return False

        self.log.debug('MySQL connection established.')
        self.cnx.autocommit = True

        return True


    def disconnect(self):
        if self.cnx:
            self.cnx.close()
        self.cnx = None
        self.log.debug('MySQL connection closed.')


    def insert(self, table, rows, args=None):
        """Saves the dictionary rows into the DB
        
        :param table: Name of the table to insert into
        :type table: str
        :param rows: A dictionary with the columns and values
        :type rows: dict
        
        :return: The return value. True for success, False otherwise.
        :rtype: bool
        
        :raise: Exception: If mysql query failed. 
        """
        cols = str(tuple(rows[0].keys())).replace("'", "`")
        self.log.debug('Columns in query: %s' % cols)

        for row in rows:
            vals = str(tuple(row.values()))
            self.log.debug('Values to insert: %s' % vals)
            sql = 'INSERT INTO ' \
                + self.conf['base'] \
                + '.' + table \
                + cols \
                + ' VALUES ( '

            for i in enumerate(rows[0].keys()):
                if i == 0: continue
                sql += '%, '

            sql += ' % );'

            self.log.info('SQL-Statement to execute: %s' % sql)
            res = self.query(sql)
            self.log.debug('SQL-Query response: %s' % str(res))

        # TODO
        return True


    def query(self, query, args=None):
        res = [ ]
        cursor = self.cnx.cursor()

        try:
            if args==None:
                cursor.execute(query)
            else:
                cursor.execute(query, args)
        except mysql.connector.Error as err:
            self.log.error("MySQL: {}".format(err))
            return None

        while True:
            x=cursor.fetchone()
            if x == None:
                break

            res.append(dict(zip(cursor.column_names, x)))

        return res


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
