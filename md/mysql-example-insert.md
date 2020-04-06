---
title: mysql example insert (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'insert'

Functions in program: 
* `def csv_to_mysql(files):`

Modules used in program: 
* `import mysql.connector`
* `import shutil`
* `import sys`
* `import glob`
* `import os`
* `import csv`

## python insert

Python mysql example: insert

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import csv
import os
import glob
import sys
import shutil
import mysql.connector

from configparser import ConfigParser


def csv_to_mysql(files):
    """
    MySQLへデータを登録する。
    速度が遅いと感じたらmultiple insertも検討する
    """
    config = ConfigParser()
    config.read('config.ini')
    conn = mysql.connector.connect(
        host=config.get('mysql', 'host'),
        port=config.getint('mysql', 'port'),
        user=config.get('mysql', 'user'),
        password=config.get('mysql', 'password'),
        database=config.get('mysql', 'database'),
        charset=config.get('mysql', 'charset'),
    )

    cursor = conn.cursor()
    for file in files:
        with open(file) as csv_file:
            reader = csv.reader(csv_file)
            for row in reader:
                try:
                    cursor.execute(
                        'insert into sensors values (%s, %s, %s, %s, %s, %s)', row)
                except:
                    conn.rollback()
                    raise
        dst = file.split('\\')
        dst.insert(-1, 'inserted')
        dst = '/'.join(dst)
        shutil.move(file, dst)
    conn.commit()
    cursor.close()
    conn.close()


if __name__ == '__main__':
    csv_to_mysql(glob.glob('*/*/*.csv'))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
