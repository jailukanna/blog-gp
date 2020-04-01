---
title: socket example simple socket db vehicle test (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'simple socket db vehicle test'


## python simple socket db vehicle test

Python socket example: simple socket db vehicle test

```python
# coding:utf-8
from db_util.connector import MySQL
from db_util import config
from db_vehicle.vehicle import VehicleStatus

"""测试之前使用如下命令进行数据库建立.
source /path/of/vehicle_init.sql;
"""

# 申请资源
db = MySQL()
# 选择数据库
db.useTable('vehicle')


if False:
    sql = 'SELECT * FROM vehicle_status WHERE datetime_ LIKE \'2016-07-11%\';'
    results = db.fetchAll(sql)
    print('*****fetchAll*****')
    print(results)
    for item in results:
        for key in item:
            print(key, '\t\t\t', item[key])
        print

if False:
    sql = 'SELECT * FROM vehicle_status WHERE datetime_ LIKE \'2016-07-11%\';'
    result = db.fetchOne(sql)
    print('*****fetchOne*****')
    print(result)
    print(result['datetime_'],)
    print(result['voltage'],)
    print(result['engin_load_abs'])

if False:
    sql = "DESCRIBE vehicle_status;"
    results = db.fetchAll(sql)

    print('*****structure of vehicle_status*****')
    if config.DB_Cursor is config.Cursor:
        for line in results:
            for key in line:
                print('%25s' % key,)
            print
    else:
        print("please set 'DB_Cursor = DictCursor' in config")


if True:
    data = (('001001', '2016-07-11 13:53:45', '50', '3000', '70', '200', '78'),
            ('001002', '2016-07-13 13:28:24', '60', '3070', '70', '240', '75'))
    VehicleStatus().insert(data)

if True:
    sql = 'SELECT id, id_device, datetime_, speed, engine_rpm, engine_load_cal, tmp_engine, tmp_water ' \
          'FROM vehicle_status;'
    results = db.fetchAll(sql)
    print(('*****All({0})*****'.format(len(results))))
    for item in results:
        print(item)

# 释放资源
db.dispose()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
