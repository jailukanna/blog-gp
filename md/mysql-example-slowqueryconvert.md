---
title: mysql example slowqueryconvert (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'slowqueryconvert'

Functions in program: 
* `def create_slow_query_log(logs):`
* `def fetch_slow_logs(host, user, password):`

Modules used in program: 
* `import sys`
* `import _mysql`
* `import argparse`

## python slowqueryconvert

Python mysql example: slowqueryconvert

```python
#!/usr/bin/env python

"""
Queries the slow_log database table maintained by Amazon RDS and outputs
it in the normal MySQL slow log text format for parsing by mk-query-
digest.
"""

import argparse
import _mysql
import sys


def fetch_slow_logs(host, user, password):
    db = _mysql.connect(host=host, user=user, passwd=password, db="mysql")
    db.query("""SELECT * FROM slow_log ORDER BY start_time""")

    r = db.use_result()
    return list(r.fetch_row(maxrows=0, how=1))


def create_slow_query_log(logs):
    sys.stdout.write("""/usr/sbin/mysqld, Version: 5.5.12-log ((Debian)). started with:
Tcp port: 3306  Unix socket: /var/run/mysqld/mysqld.sock
Time                 Id Command    Argument
""")

    for log in logs:
        log['year'] = log['start_time'][2:4]
        log['month'] = log['start_time'][5:7]
        log['day'] = log['start_time'][8:10]
        log['time'] = log['start_time'][11:]

        hours = int(log['query_time'][0:2])
        minutes = int(log['query_time'][3:5])
        seconds = int(log['query_time'][6:8])
        log['query_time_f'] = hours * 3600 + minutes * 60 + seconds

        hours = int(log['lock_time'][0:2])
        minutes = int(log['lock_time'][3:5])
        seconds = int(log['lock_time'][6:8])
        log['lock_time_f'] = hours * 3600 + minutes * 60 + seconds

        if not log['sql_text'].endswith(';'):
            log['sql_text'] += ';'

        sys.stdout.write('# Time: {year}{month}{day} {time}\n'.format(**log))
        sys.stdout.write('# User@Host: {user_host}\n'.format(**log))
        sys.stdout.write('# Query_time: {query_time_f}  Lock_time: {lock_time_f} Rows_sent: {rows_sent}  Rows_examined: {rows_examined}\n'.format(**log))
        sys.stdout.write('use {db};\n'.format(**log))
        sys.stdout.write(log['sql_text'] + '\n')


if __name__ == '__main__':

    parser = argparse.ArgumentParser()
    parser.add_argument('-H', '--host', help="The DB host to connect to", default="localhost")
    parser.add_argument('-u', '--user', help="The user to connect with", default="root")
    parser.add_argument('-p', '--password', help="The password to connect with", default="")

    args = parser.parse_args()

    slow_logs = fetch_slow_logs(host=args.host, user=args.user, password=args.password)

    create_slow_query_log(slow_logs)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
