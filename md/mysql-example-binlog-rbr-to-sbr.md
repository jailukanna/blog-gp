---
title: mysql example binlog-rbr-to-sbr (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'binlog-rbr-to-sbr'

Functions in program: 
* `def convert_rbr_to_pseudo_sbr(db):`
* `def replace_column(m):`

Modules used in program: 
* `import datetime`
* `import time`
* `import mysql.connector`
* `import collections`
* `import getopt`
* `import re`
* `import sys`
* `import fileinput`

## python binlog-rbr-to-sbr

Python mysql example: binlog-rbr-to-sbr

```python
#!/usr/bin/python
#
# Convert a Row-Based-Replication binary log to Statement-Based-Replication format, cheating a little.
# 
# Additional added some code to transfer INSERT clauses to proper column names.
#
# Usage: mysqlbinlog -v --base64-output=DECODE-ROWS $log | python binlog-rbr-to-sbr.py -hHOST -uUSERNAME -pPASSWORD - | grep -B 1 '^INSERT '
# Based on: https://gist.github.com/shlomi-noach/cc243fd690403e7617e3
#

import fileinput
import sys
import re
import getopt
import collections
import mysql.connector
import time
import datetime

table_cache = {}
current_table = None

def replace_column(m):
    global table_cache
    global current_table

    if current_table in table_cache:
        index = int(m.group(2)) - 1
        sep = ', ' if index > 0 else ''
        column_name = table_cache[current_table].keys()[index]
        column_type = table_cache[current_table][column_name]
        if 'timestamp' in column_type:
            tstamp = datetime.datetime.fromtimestamp(int(m.group(3)))
            return (u'%s`%s` = \'%s\'' % (sep, column_name, tstamp.strftime('%Y-%m-%d %H:%M:%S')))
        return (u'%s`%s` = %s' % (sep, column_name, m.group(3)))
    return m.group(1)

def convert_rbr_to_pseudo_sbr(db):
    global table_cache
    global current_table

    inside_rbr_statement = False
    is_insert_statement = False

    for line in fileinput.input():
        line = line.strip().decode("utf8")
        if line.startswith("#") and "end_log_pos" in line:
            for rbr_token in ["Update_rows:", "Write_rows:", "Delete_rows:", "Rows_query:", "Table_map:",]:
                if rbr_token in line:
                    line = "%s%s" % (line.split(rbr_token)[0], "Query\tthread_id=1\texec_time=0\terror_code=0")
        if line.startswith("### "):
            inside_rbr_statement = True
            # The "### " commented rows are the pseudo-statement interpreted by mysqlbinlog's "--verbose",
            # and which we will feed into pt-query-digest
            line = line.split(" ", 1)[1].strip()
            if line.upper().startswith('INSERT'):
                is_insert_statement = True
                match = re.search('^INSERT INTO ([^ ]+)', line)
                if match:
                    current_table = match.group(1)
                    if current_table not in table_cache:
                        cursor = db.cursor()
                        query = 'DESCRIBE %s' % current_table
                        cursor.execute(query)
                        table_cache[current_table] = collections.OrderedDict()
                        for col in cursor:
                            table_cache[current_table][col[0]] = col[1]
        else:
            if inside_rbr_statement:
                print("/*!*/;")
            is_insert_statement = False
            inside_rbr_statement = False
            current_table = None
        if line.startswith('#'):
            print(line.encode("utf8"))
        else:
            if is_insert_statement:
                if current_table in table_cache:
                    orig_line = line
                    line = re.sub('(@([0-9]+)=(.+))', replace_column, line)
            sys.stdout.write(line.encode("utf8"))
            sys.stdout.write(" ")
        
optlist, args = getopt.getopt(sys.argv[1:], 'h:u:p:')

mysql_host = None
mysql_username = None
mysql_password = None
for opt in optlist:
    key, val = opt
    if key == '-h':
        mysql_host = val
    if key == '-u':
        mysql_username = val
    if key == '-p':
        mysql_password = val

if not mysql_host or not mysql_username or not mysql_password:
    print('Usage: binlog-rbr-to-sbr.py -hHOST -uUSERNAME -pPASSWORD')
    sys.exit(1)

sys.argv = args

cnx = mysql.connector.connect(user=mysql_username, password=mysql_password,
                              host=mysql_host)

convert_rbr_to_pseudo_sbr(cnx)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
