---
title: mysql example default controller (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'default controller'

Functions in program: 
* `def find_measures(device_id, limit):  # noqa: E501`
* `def find_measure(device_id):  # noqa: E501`
* `def createConnection():`

Modules used in program: 
* `import mysql.connector`
* `import six`
* `import connexion`

## python default controller

Python mysql example: default controller

```python
import connexion
import six
# using MySQL connector
import mysql.connector
from swagger_server.controllers import dbconfig as cfg

from swagger_server.models.measure import Measure  # noqa: E501
from swagger_server import util

def createConnection():
    # MySQL Environment Parameters read from dbconfig.py
    # using connection pool
    
    conn = mysql.connector.connect(user=cfg.mysql['user'], password=cfg.mysql['passwd'],
                              host=cfg.mysql['host'],
                              database=cfg.mysql['db'],
                              pool_name = "proxy_pool",
                              pool_size = 5) # default size
    return conn

def find_measure(device_id):  # noqa: E501
    """retrieve the last measure of Air Conditions
     # noqa: E501
    :param device_id: the id of the device (id_list)
    :type device_id: int
    :rtype: Measure
    """
    conn = createConnection()

    print("Connection created...")

    cur = conn.cursor()

    query = ('SELECT id, ts_op, lux, temperature, humidity, pressure, iaq, tvoc, co2e, pm2_5, pm10, hflevel, lflevel'
            ' FROM dev_aircare_data WHERE id_list = %(device_id)s AND id = (SELECT MAX(id) FROM dev_aircare_data WHERE id_list = %(device_id)s)')
    
    # selecting a single device
    cur.execute(query, {"device_id": device_id})
    
    # we're sure that the query returns a single record
    
    # this way we avoid error if device_id is not existing in table
    ms = {}

    for (id, ts_op, lux, temperature, humidity, pressure, iaq, tvoc, co2e, pm2_5, pm10, hflevel, lflevel) in cur:
        ms = Measure(id, ts_op, float(lux), float(temperature), float(humidity), float(pressure), float(iaq), int(tvoc), 
            int(co2e), int(pm2_5), int(pm10), float(hflevel), int(lflevel)) 

    cur.close()
    conn.close()

    print("Connection closed...")

    return ms


def find_measures(device_id, limit):  # noqa: E501
    """List all latest measurements of Air conditions
     # noqa: E501
    :param device_id: the id of the device (id_list))
    :type device_id: int
    :param limit: max number of record retrieved
    :type limit: int
    :rtype: List[Measure]
    """
    
    conn = createConnection()

    print("Connection created...")

    cur = conn.cursor()

    query = ('SELECT id, ts_op, lux, temperature, humidity, pressure, iaq, tvoc, co2e, pm2_5, pm10, hflevel, lflevel'
            ' FROM dev_aircare_data WHERE id_list = %(device_id)s ORDER BY id DESC LIMIT %(limit)s')

    cur.execute(query, {"device_id": device_id, "limit": limit})

    # the structure to return data
    vetResult = []

    for (id, ts_op, lux, temperature, humidity, pressure, iaq, tvoc, co2e, pm2_5, pm10, hflevel, lflevel) in cur:
        # build a single Measure
        ms = Measure(id, ts_op, float(lux), float(temperature), float(humidity), float(pressure), float(iaq), int(tvoc), 
            int(co2e), int(pm2_5), int(pm10), float(hflevel), int(lflevel))
        # add ms to the array
        vetResult.append(ms)

    cur.close()
    conn.close()

    print("Connection closed...")

    return vetResult


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
