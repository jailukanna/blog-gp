---
title: mysql example migrate encoding (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'migrate encoding'

Functions in program: 
* `def start(host, port, user, passwd, db, charset, encoding):`
* `def execute(conn, query):`

Modules used in program: 
* `import argparse`
* `import _mysql`
* `import getpass`

## python migrate encoding

Python mysql example: migrate encoding

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
import getpass
import _mysql
import argparse


def execute(conn, query):
    conn.query(query)
    result = conn.store_result()
    if result:
        return result.fetch_row(maxrows=0, how=1)
    return result


def start(host, port, user, passwd, db, charset, encoding):
    script = """ """
    conn = _mysql.connect(user=user, passwd=passwd, host=host, port=port, db=db)
    # first we alter database
    q = """ALTER DATABASE `{database}` CHARACTER SET = {charset} COLLATE = {encoding};\r\n""".format(**{
        'database': db,
        'charset': charset,
        'encoding': encoding,
    })
    script += q
    # retreive table names
    q = """SELECT T.table_name FROM information_schema.`TABLES` T WHERE T.table_schema = '{}'\r\n""".format(db)
    result = execute(conn=conn, query=q)
    tablenames = [r['table_name'] for r in result]
    # alter each table name
    for t in tablenames:
        q = """ALTER TABLE `{table_name}` CONVERT TO CHARACTER SET {charset} COLLATE {encoding};\r\n""".format(**{
            'table_name': t,
            'charset': charset,
            'encoding': encoding,
        })
        script += q
    # retreive and convert each column
    for t in tablenames:
        q = """ SHOW FULL COLUMNS FROM {}""".format(t)
        columns = execute(conn=conn, query=q)
        # for each column check format and alter it
        for col in columns:
            if col['Collation'] is not None:
                print('before : {} {} {} {} default {}'.format(t, col['Field'], col['Type'], col['Collation'], col['Default']))
                default = None
                if col['Default'] is None:
                    default = 'NULL'
                else:
                    default = "'{}'".format(col['Default'])
                q = """ALTER TABLE `{table_name}` CHANGE `{column_name}` `{column_name}`
                {column_type} CHARACTER SET {charset} COLLATE {encoding} DEFAULT {default};\r\n
                """.format(**{
                    'table_name': t,
                    'column_name': col['Field'],
                    'column_type': col['Type'],
                    'charset': charset,
                    'encoding': encoding,
                    'default': default
                })
                script += q
    with open('migrate.sql', 'wb') as f:
        f.write(script)
    print("success")


if __name__ == '__main__':
    a = argparse.ArgumentParser(description="Migrate MYSQL encoding SQL script generator")
    a.add_argument("-H", "--host", help="MYSQL database host", required=True)
    a.add_argument("-p", "--port", help="MYSQL database port", nargs='?', const=3306, type=int, default=3306)
    a.add_argument("-u", "--user", help="MYSQL database username", required=True)
    a.add_argument("-d", "--db", help="MYSQL database name", required=True)
    a.add_argument("-c", "--charset", help="MYSQL charset name", required=False, default='utf8mb4')
    a.add_argument("-e", "--encoding", help="MYSQL table field encoding", required=False, default='utf8mb4_unicode_ci')
    args = a.parse_args()
    display = """
    host      {host}
    port      {port}
    database  {db}
    username  {user}
    charset   {charset}
    encoding  {encoding}
    """.format(**vars(args))
    print(display)
    args.passwd = getpass.getpass()
    start(**vars(args))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
