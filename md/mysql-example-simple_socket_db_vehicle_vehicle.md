---
title: mysql example simple socket db vehicle vehicle (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'simple socket db vehicle vehicle'


## python simple socket db vehicle vehicle

Python mysql example: simple socket db vehicle vehicle

```python
# coding=utf-8
from db_util.connector import MySQL


class __VehicleBase(object):
    INSERT = 0
    UPDATE = 1
    __insert = ''
    __update = ''

    def __init__(self, sql_insert, sql_update):
        self.__insert = sql_insert
        self.__update = sql_update

    def insert(self, data_tuple):
        self.__action(self.INSERT, data_tuple)

    def update(self, data_tuple):
        self.__action(self.UPDATE, data_tuple)

    def __action(self, action, data_tuple):
        db = None
        try:
            # 申请资源
            db = MySQL()
            # 选择数据库
            db.useTable('vehicle')
            if action == self.INSERT:
                rows = db.insert(self.__insert, data_tuple)
                print(('insert: {0} lines affected'.format(rows)))
            elif action == self.UPDATE:
                rows = db.update(self.__update, data_tuple)
                print(('update: {0} lines affected'.format(rows)))

        except Exception, e:
            print("----Error: unable to fetch data")
            print(self.__insert)
            print(data_tuple)
            print(e.args)

        finally:
            # 释放资源
            db.dispose()


class VehicleStatus(__VehicleBase):

    sql_insert = 'INSERT INTO vehicle_status' \
                 '(id_device, datetime_, speed, engine_rpm, engine_load_cal, tmp_engine, tmp_water) ' \
                 'VALUE (%s, %s, %s, %s, %s, %s, %s);'

    sql_update = ''

    def __init__(self):
        super(VehicleStatus, self).__init__(self.sql_insert, self.sql_update)


class VehicleHistory(__VehicleBase):
    sql_insert = 'INSERT INTO vehicle_history' \
                 '(id_device, datetime_, speed, engine_rpm, engine_load_cal, tmp_engine, tmp_water) ' \
                 'VALUE (%s, %s, %s, %s, %s, %s, %s);'

    sql_update = ''

    def __init__(self):
        super(VehicleHistory, self).__init__(self.sql_insert, self.sql_update)


class VehicleInfo(__VehicleBase):
    sql_insert = 'INSERT INTO vehicle_info' \
                 '(id_device, datetime_, speed, engine_rpm, engine_load_cal, tmp_engine, tmp_water) ' \
                 'VALUE (%s, %s, %s, %s, %s, %s, %s);'

    sql_update = ''

    def __init__(self):
        super(VehicleInfo, self).__init__(self.sql_insert, self.sql_update)




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
