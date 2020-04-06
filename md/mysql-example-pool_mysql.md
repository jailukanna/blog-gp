---
title: mysql example pool mysql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'pool mysql'

Functions in program: 
* `def worker_wrapper(data):`
* `def initializer(mysql_config):`

Modules used in program: 
* `import logging`
* `import mysql.connector  # works with pypy`

## python pool mysql

Python mysql example: pool mysql

```python
from multiprocessing import Pool
import mysql.connector  # works with pypy
import logging

mysql_config = {}

def initializer(mysql_config):
    """ One connection per process, setup by Pool()."""
    global cnx
    global cur
    logging.config.fileConfig('logging.ini')
    cnx = mysql.connector.connect(**mysql_config)
    cur = cnx.cursor()


def worker_wrapper(data):
    """ Each process commits separately. """
    global cnx
    global cur
    sequential_worker(cur, data)
    cnx.commit()

# Initialize 8 processes with their own mysql connection.
pool = Pool(8, initializer=initializer, initargs=(mysql_config,))
# Process in parallel my long dataset.
pool.map(worker_wrapper, dataset)
pool.close()
pool.join()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
