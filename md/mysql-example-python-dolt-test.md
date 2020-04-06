---
title: mysql example python-dolt-test (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'python-dolt-test'

Functions in program: 
* `def start_dolt_sql_server(directory):`

Modules used in program: 
* `import mysql.connector`
* `import subprocess`
* `import time`
* `import sys`

## python python-dolt-test

Python mysql example: python-dolt-test

```python
# This is a sample python script which reads from dolt and prints the output.
#
# requirements: Install the python mysql connector
#    python -m pip install mysql-connector-python
#
# usage: python python-dolt-test.py <dolt directory> <query>
#
# <dolt directory> is the directory of an existing dolt repository where dolt sql-server
# will be started.  dolt sql-server will use the current branch.
#
# <query> a valid sql query

import sys
import time
import subprocess
import mysql.connector

dolt_repo_dir = sys.argv[1]
query_str = sys.argv[2]

def start_dolt_sql_server(directory):
    proc = subprocess.Popen(args=['dolt', 'sql-server', '-t', '0'], cwd=directory, stdout=subprocess.PIPE, stderr=subprocess.STDOUT)
    # should probably start a thread to watch this or something.
    
    # hack: need to wait for the process to get going otherwise the connection will fail
    print('waiting for server to start')
    time.sleep(2)
    
    return proc

server_proc = start_dolt_sql_server(dolt_repo_dir)

cnx = mysql.connector.connect(user='root', host='127.0.0.1', port=3306, database='dolt')
cursor = cnx.cursor()
cursor.execute(query_str)

for row in cursor:
    print(' | '.join([str(col_val) for col_val in row]))

server_proc.kill()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
