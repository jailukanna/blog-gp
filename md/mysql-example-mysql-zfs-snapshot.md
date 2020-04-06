---
title: mysql example mysql-zfs-snapshot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql-zfs-snapshot'


Modules used in program: 
* `import sys`
* `import subprocess`
* `import mysql.connector`

## python mysql-zfs-snapshot

Python mysql example: mysql-zfs-snapshot

```python
#!/usr/bin/env python
#
# ./$scriptname zroot/var/mysql@$(date +"%FT%H:%M")
#
# Dependencies for FreeBSD:
# pkg install py27-mysql-connector-python2

import mysql.connector
import subprocess
import sys

cnx = mysql.connector.connect(user='root', unix_socket='/var/run/mysqld/mysqld.sock')
cursor = cnx.cursor()
cursor.execute('FLUSH TABLES WITH READ LOCK')
subprocess.call(['zfs', 'snapshot', sys.argv[1]])
cursor.execute('UNLOCK TABLES')
cnx.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
