---
title: mysql example mysql dump (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql dump'

Functions in program: 
* `def dump_table(table):`
* `def get_select(table, limit, offset):`
* `def get_count(table):`
* `def get_fields(table):`

Modules used in program: 
* `import math`
* `import gzip`
* `import csv`
* `import mysql.connector as myconn`

## python mysql dump

Python mysql example: mysql dump

```python
import mysql.connector as myconn
import csv
import gzip
import math

cnx = myconn.connect(user='root',
                     host='127.0.0.1',
                     database='adapta_aps')

blacklist = [u'DATABASECHANGELOG', u'DATABASECHANGELOGLOCK', u'QRTZ_BLOB_TRIGGERS', u'QRTZ_CALENDARS', u'QRTZ_CRON_TRIGGERS', u'QRTZ_FIRED_TRIGGERS', u'QRTZ_JOB_DETAILS', u'QRTZ_LOCKS', u'QRTZ_PAUSED_TRIGGER_GRPS', u'QRTZ_SIMPLE_TRIGGERS', u'QRTZ_SIMPROP_TRIGGERS', u'QRTZ_TRIGGERS']


def get_fields(table):
    list_columns_statement = """
        SHOW COLUMNS FROM {0}
    """.format(table)
    cursor = cnx.cursor()
    cursor.execute(list_columns_statement)
    return [record[0] for record in cursor]


def get_count(table):
    count_records_statement = """
        SELECT COUNT(1) FROM {0}
    """.format(table)
    cursor = cnx.cursor()
    cursor.execute(count_records_statement)
    return [record[0] for record in cursor][0]


def get_select(table, limit, offset):
    select_records_statement = """
        SELECT * FROM {0} LIMIT {1} OFFSET {2}
    """.format(table, offset, limit)
    cursor = cnx.cursor()
    cursor.execute(select_records_statement)
    return [list(record) for record in cursor]
    

def dump_table(table):
    if (table in blacklist):
        return
    
    fields = get_fields(table)
    count = get_count(table)
    print("{0} records".format(count))
    step = int(math.ceil(count / 100)) + 1;
    with gzip.open("data/{0}.csv.gz".format(table), "wb") as f:
        writer = csv.writer(f)
        writer.writerow(fields)
        for i in xrange(0, count, step):       
            records = get_select(table, step, i)
            map(writer.writerow, records)
    

list_tables_statement = """
    SHOW TABLES
"""


cursor = cnx.cursor()
cursor.execute(list_tables_statement)

tables = [record[0] for record in cursor]
for table in tables:
    print("Processing table {0}".format(table))
    dump_table(table)
    

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
