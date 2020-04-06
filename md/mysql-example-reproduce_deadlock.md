---
title: mysql example reproduce deadlock (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'reproduce deadlock'

Functions in program: 
* `def insert(x):`
* `def create_table():`
* `def execute_statement(statement):`
* `def create_mysql_container():`
* `def get_connection():`

Modules used in program: 
* `import random`
* `import sys`
* `import time`
* `import mysql.connector`
* `import docker`

## python reproduce deadlock

Python mysql example: reproduce deadlock

```python
import docker
import mysql.connector
import time
from multiprocessing import Pool
import sys
import random

create_table_statement = '''
CREATE TABLE test.`data` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `col1` int(10) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `somekey` (`col1`)
) ENGINE=InnoDB;
'''
version=sys.argv[1]

get_port_for_version = lambda x: int(x.split(".")[-1] + "06")

def get_connection():
    conn = mysql.connector.connect(
      host="127.0.0.1",
      user="root",
      passwd="root",
      port=get_port_for_version(version),
      connection_timeout = 5
    );
    conn.autocommit = True
    return conn

def create_mysql_container():
    client = docker.from_env()
    container = client.containers.run(image="mysql:" + version, environment={"MYSQL_ROOT_PASSWORD": "root"}, ports = {"3306/tcp" : get_port_for_version(version)}, detach=True)
    print("let container start")
    time.sleep(10)
    return container

def execute_statement(statement):
    try:
        conn = get_connection()
        cur = conn.cursor()
        cur.execute(statement)
        cur.close()
        conn.close()
    except Exception as e:
        print(e)

def create_table():
    execute_statement("create database test;")
    execute_statement(create_table_statement)

def insert(x):
    p1 = random.randint(1, 101)
    p2 = random.randint(1, 101)
    return execute_statement("insert ignore into test.`data` ( `col1`) values ({0}), ({1});".format(p1, p2))

container = create_mysql_container()
create_table()
with Pool(5) as p:
    p.map(insert, range(1000))
container.kill()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
