---
title: mysql example restarter (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'restarter'

Functions in program: 
* `def update():`
* `def get_status():`
* `def find_process(name):`
* `def start_process(name):`

Modules used in program: 
* `import datetime`
* `import mysql.connector`
* `import os`
* `import psutil`
* `import time`
* `import sched`

## python restarter

Python mysql example: restarter

```python
import sched
import time
import psutil
import os
import mysql.connector
import datetime

####################
# DATABASE CONFIG
####################
DB_HOST = '127.0.0.1'
DB_USER = 'trinity'
DB_PASSW = 'trinity'
DB_DATABASE = 'auth'

####################
# REALM CONFIG
####################
PROCESS_NAME = 'worldserver.exe'
REALM_NAME = 'Test Server'

####################
# APPLICATION CODE
####################
DB = mysql.connector.connect(
    host=DB_HOST,
    user=DB_USER,
    passwd=DB_PASSW,
    database=DB_DATABASE
)

DB.autocommit = True
DB.connect()

PROCESS_RUNNING = None
REALM_INITIALIZED = False


def start_process(name):
    try:
        os.startfile(name)
    except FileNotFoundError:
        print(f'Cannot find {os.getcwd()}\\{name}')
        print('Please place run.py into your TrinityCore directory!')
        exit(1)


def find_process(name):
    for p in psutil.process_iter():
        if p.name() == name:
            return True

    return False


def get_status():
    date_time = datetime.datetime.now().strftime("[%H:%M:%S]")

    global REALM_INITIALIZED
    cursor = DB.cursor()
    cursor.execute('SELECT flag FROM realmlist WHERE realmlist.name = %s',
                   (REALM_NAME,))
    result = cursor.fetchone()

    # 0x2 = REALM_FLAG_OFFLINE
    # realm_status = 1 = True  = Online
    #              = 0 = False = Offline
    realm_status = not (result[0] & 0x2)

    if realm_status != REALM_INITIALIZED:
        REALM_INITIALIZED = realm_status
        if REALM_INITIALIZED:
            print(f'{date_time} Realm {REALM_NAME} is now Online!')
        else:
            print(f'{date_time} Realm {REALM_NAME} is now Offline!')


def update():
    date_time = datetime.datetime.now().strftime("[%H:%M:%S]")
    # Check DB if realm is online
    get_status()

    global PROCESS_RUNNING
    if PROCESS_RUNNING is None:
        start_process(PROCESS_NAME)
        print(f'{date_time} Starting {PROCESS_NAME}!')

    if find_process(PROCESS_NAME):
        if not PROCESS_RUNNING:
            PROCESS_RUNNING = True
            print(f'{date_time} {PROCESS_NAME} running!')
    else:
        if PROCESS_RUNNING:
            PROCESS_RUNNING = False
            print(f'{date_time} {PROCESS_NAME} stopped! Restarting ...')
            start_process(PROCESS_NAME)
    global s
    s.enter(1, 1, update)


if __name__ == "__main__":
    s = sched.scheduler(time.time, time.sleep)
    s.enter(1, 1, update)
    s.run()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
