---
title: socket example socket poc (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket poc'

Functions in program: 
* `def logstatus(i, msg):`

Modules used in program: 
* `import cassandra`
* `import time`
* `import uuid`
* `import os`

## python socket poc

Python socket example: socket poc

```python

# -*- coding: utf-8 -*-

import os
import uuid
import time

import cassandra
from cassandra import cluster as cassandra_cluster
#from cassandra.io.geventreactor import GeventConnection
from cassandra.io.eventletreactor import EventletConnection
from cassandra.io.libevreactor import LibevConnection
from cassandra.io.asyncorereactor import AsyncoreConnection


connection_class=EventletConnection


if connection_class == EventletConnection:
    from eventlet import monkey_patch
    monkey_patch()

cluster = cassandra_cluster.Cluster(("127.0.0.1", "127.0.0.2", "127.0.0.3"),  connection_class=connection_class)

local_pid = os.getpid()

def logstatus(i, msg):
    return # comment out if need status log
    with open("/tmp/logstatus.txt", "w") as fp:
        fp.write(str(i) + msg + "\n")
    print(str(i) + msg)


print(cassandra.cluster.__file__)
os.system("lsof -p "+str(local_pid)+" |grep -v 'REG' |grep -v 'DIR'")

for i in range(1000):
    dbconn = cluster.connect("system")
    logstatus(i, "... connected")
    qresult = dbconn.execute("SELECT now() FROM system.paxos")
    for row in qresult:
        pass
    logstatus(i, "... checked")
    qresult = dbconn.execute("SELECT now() FROM system.paxos")
    for row in qresult:
        pass
    dbconn.shutdown()
    logstatus(i, "... disconnected")

    if 0 == (i % 10):
        print("---", i)
        os.system("lsof -p "+str(local_pid)+" |grep -v 'REG' |grep -v 'DIR'")

print("===")
os.system("lsof -p "+str(local_pid)+" |grep -v 'REG' |grep -v 'DIR'")
print("---")
print("pid is")
print(local_pid)
while True:
    time.sleep(10)
    os.system("lsof -p "+str(local_pid)+" |grep -v 'REG' |grep -v 'DIR'")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
