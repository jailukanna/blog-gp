---
title: mysql example perf (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'perf'

Functions in program: 
* `def main():`
* `def print_usage():`
* `def reload_dict(rows):`
* `def print_rows(rows):`
* `def insert_rows(rows, dict):`
* `def fetch_rows():`
* `def fetch_test():`
* `def group_results(table, key, value):`
* `def itemgetter(keys, rowtype=tuple):`

Modules used in program: 
* `import json`
* `import MySQLdb.constants.CLIENT as CL`
* `import MySQLdb`
* `import _mysql`
* `import datetime`
* `import time`
* `import sys, re`

## python perf

Python mysql example: perf

```python
#!/usr/bin/env python
# encoding: utf-8

"""
perf.py - Aggregates rows to minute interval
by fieds: project, script, context, container, mode, host
summing fields: sum, cnt, min, max, avg

NOTE! ONLY cron usage with 5 min interval. 

Created by Denis Bondar.
"""

import sys, re
import time
import datetime
import _mysql
import MySQLdb
import MySQLdb.constants.CLIENT as CL
import json
from itertools import groupby, imap
from pprint(import pprint(as pp)

Error = MySQLdb.DatabaseError

'''
USE perf2;
DROP TABLE `timerlog`;
DROP TABLE `timerlog_dict`;
CREATE TABLE `timerlog_dict` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `project` char(64) DEFAULT NULL,
  `script` char(64) DEFAULT NULL,
  `context` char(64) DEFAULT NULL,
  `container` char(64) DEFAULT NULL,
  `mode` char(64) DEFAULT NULL,
  `hostname` char(64) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `mult_key` (`project`,`script`,`context`,`container`,`mode`,`hostname`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `timerlog` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `dict_id` int(10) unsigned NOT NULL,
  `ts` int(10) unsigned NOT NULL DEFAULT '0',
  `sum` int(10) unsigned NOT NULL DEFAULT '0',
  `cnt` int(10) unsigned NOT NULL DEFAULT '0',
  `min` int(10) unsigned NOT NULL DEFAULT '0',
  `max` int(10) unsigned NOT NULL DEFAULT '0',
  `avg` int(10) unsigned NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`),
  FOREIGN KEY (dict_id) REFERENCES timerlog_dict(id) ON DELETE CASCADE,
  KEY `dict_ts` (`dict_id`,`ts`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
'''

""" Main SQL to fetch raw rows """
select_sql = """SELECT t.id as id,
                    (ROUND(t.ts/60)*60) as ts,
                    t.sum as sum,
                    t.cnt as cnt,
                    t.min as min,
                    t.max as max,
                    t.avg as avg,
                    NULLIF(hs.hostname,'-') as hostname,
                    NULLIF(hs.script,'-') as script,
                    group_concat(tv.tag order by tv.tag separator ',') as tag,
                    group_concat(tv.value order by tv.tag separator ',') as value
FROM perf.timers t, perf.hostscript hs, perf.timertags tg, perf.tag_values tv
WHERE t.ts >=UNIX_TIMESTAMP(DATE_FORMAT(NOW() - INTERVAL 5 MINUTE, '%Y-%m-%d %H:%i')) AND t.ts<UNIX_TIMESTAMP(DATE_FORMAT(NOW(), '%Y-%m-%d %H:%i'))
         AND t.hsid=hs.id
         AND t.id=tg.timer_id
         AND tg.tag_value_id=tv.id
GROUP BY t.id
"""

insert_sql = """INSERT INTO perf2.timerlog (ts, dict_id, sum, cnt, min, max, avg)
VALUES (%s, %s, %s, %s, %s, %s, %s)
"""

# Note! THE ORDER IS IMPORTANT
select_dict_sql = """SELECT id, CONCAT_WS(',',project, script, container, context, hostname, mode) as con FROM perf2.timerlog_dict"""

# Note! THE ORDER IS IMPORTANT
insert_dict_sql = """INSERT IGNORE INTO perf2.timerlog_dict (project, script, container, context, hostname, mode)
VALUES ('%s', '%s', '%s', '%s', '%s', '%s')
"""

class MySQL:
    _conn = None
    host = "127.0.0.1"
    unix_socket = "/var/run/mysqld/mysqld.sock"
    username = "root"
    password = ""
    port = 3306

    def __init__(self, init_connect=None):
        self.init_connect = init_connect

    """ Basic MySQL connection functionality """
    def reconnect(self):
        self._conn = None

        while self._conn == None:
            try:
                self._conn = _mysql.connect(
                    unix_socket=self.unix_socket,
                    user=self.username,
                    passwd=self.password,
                    init_command="SET SESSION wait_timeout=5",
                    client_flag=CL.MULTI_STATEMENTS | CL.MULTI_RESULTS
                )
                if self.init_connect:
                    self.q(self.init_connect)
            except MemoryError:
                print(sys.exc_info())
                sys.exit(1)
            except MySQLdb.Error:
                print(sys.exc_info())
                time.sleep(1)

    def q(self, query, use_result=True):
        # Establish connection if not existing or fails to ping
        if not self._conn:
            self.reconnect()

        # We will retry just once - reconnect has infinite loop though
        for attempt in (True, False):
            try:
                self._conn.query(query)
                break  # if successful
            except MySQLdb.OperationalError:
                if attempt:
                    self.reconnect()
                    continue
                else:
                    return None

        if use_result:
            ret = []
            while True:
                r = self._conn.store_result()
                if not r:
                    # Certain statement sequences will
                    # produce multiple NULL resultsets,
                    # so we have to handle these properly
                    # ^^ - this is not a Haiku
                    if self._conn.next_result() < 0:
                        break
                    else:
                        continue

                while True:
                    row = r.fetch_row(how=1)
                    if row:
                        ret.append(row[0])
                    else:
                        break

            return ret
        else:
            return

def itemgetter(keys, rowtype=tuple):
	""" Custom itemgetter for multiple keys """
    def getitem(value):
        return rowtype(value[key] for key in keys)
    return getitem

def group_results(table, key, value):
	""" Grouping by list of keys summing by list of values """
    get_key = itemgetter(key)
    get_value = itemgetter(value)
    grouped = {}
    for row in table:
        key = get_key(row)
        value = get_value(row)
        if key in grouped:
            grouped[key] = tuple(a + b for a, b in zip(grouped[key], value))
        else:
            grouped[key] = value
    return [k + v for k, v in sorted(grouped.iteritems())]

def fetch_test():
    tuples = [
        (1340373840, 'socdwar.ru', '/entry_point.php', 'user|tutorial_step', 'node', 'read2', 'socdwar10', 1,2,3,4,5),
        (1340373840, 'socdwar.ru', '/entry_point.php', 'user|tutorial_step', 'node', 'read', 'socdwar10', 1,2,3,4,5),
        (1340373840, 'socdwar.ru', '/entry_point.php', 'user|tutorial_step', 'node', 'read', 'socdwar10', 1,2,3,4,5),
        (1340373840, 'socdwar.ru', '/private/fsfeedback.php', None, 'db', 'read', 'socdwar15', 1,2,3,4,5),
        (1340373840, 'socdwar.ru', '/private/fsfeedback.php', None, 'db', 'read', 'socdwar7', 1,2,3,4,5),
    ]
    return tuples

#@profile
def fetch_rows():
    """Returns a dict consist of logs and dictionary (project, script, container, context, hostname, mode)."""
    item = {
        'logs': [],
        'dict': []
    }
    rows = MySQL().q(select_sql)
    for row in rows:
        _con_values = [(s or '-') for s in row['value'].split(',')]
        _values = "%s,%s,%s" % ((row['hostname'] or '-'), (row['script'] or '-'), ','.join(_con_values))
        tup_dict = tuple(_values.split(','))
        if tup_dict not in item['dict']:
            item['dict'].append(tup_dict)
            
        tup_log = (int(row['ts']), _values, int(row['sum']), int(row['cnt']), int(row['min']), int(row['max']), int(row['avg']))
        item['logs'].append(tup_log)
    
    return item

def insert_rows(rows, dict):
    """Inserts a list of tuples."""
    for row in rows:
        values = list(row)
        values[1] = int(dict[values[1]])
        MySQL().q(insert_sql % tuple(values), False)

def print_rows(rows):
    for t in rows:
        #print("(%s, %s, %s, %s, %s, %s, %s, %s)," % (ts, hostname, script, context, container, mode, host, total))
        print(t)

def reload_dict(rows):
    """Insert a dictionary and return frash copy of it."""
    for row in rows:
        #print(insert_dict_sql % row)
        MySQL().q(insert_dict_sql % row, False)
    
    items = MySQL().q(select_dict_sql)
    log_dict = {}
    for item in items:
        log_dict[item['con']] = item['id']
    return log_dict

def print_usage():
    print("""usage: ./%s cron)
insert rows from last 5 min
    """ % sys.argv[0]
    sys.exit(1)

def main():
    args = sys.argv[1:]

    if not args:
        print_usage()

    if args[0] != 'cron':
        print_usage()

	# Fetch raw data
    rows = fetch_rows ()
    print("Rows log:", len(rows['logs']))
    print("Rows dict", len(rows['dict']))

	# Update dictionary (project, script, container, context, hostname, mode)
    rows['dict'] = reload_dict(rows['dict'])

	# Group by keys summing values
    grouped = list(group_results(rows['logs'], [0,1], [2,3,4,5,6]))
    print("Rows:", len(grouped))

	# Insert grouped rows using dictionary
    insert_rows (grouped, rows['dict'])
    
if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
