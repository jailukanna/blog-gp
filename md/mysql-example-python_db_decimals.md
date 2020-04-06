---
title: mysql example python db decimals (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'python db decimals'


Modules used in program: 
* `import mysql.connector`
* `import MySQLdb`
* `import pymysql`

## python python db decimals

Python mysql example: python db decimals

```python
create_table = """
DROP TABLE IF EXISTS `dec_test`;
CREATE TABLE `dec_test` (
  `dec_2_2` decimal(4,2),
  `dec_4_2` decimal(6,2),
  `dec_8_4` decimal(8,4),
  `char_15` char(15)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

insert into dec_test values
(1, 1, 1, 1),
(1.1, 1.1, 1.1, 1.1),
(1.11, 1.11, 1.11, 1.11),
(1.111, 1.111, 1.111, 1.111),
(1.1111, 1.1111, 1.1111, 1.1111),
(1.11111, 1.11111, 1.11111, 1.11111),
(11.11111, 11.11111, 11.11111, 11.11111),
(NULL, 111.11111, 111.11111, 111.11111),  -- too large values cause error
(NULL, 1111.11111, 1111.11111, 1111.11111),
(NULL, NULL, NULL, 11111.11111)
;
"""

# pymysql
import pymysql

conn = pymysql.connect(host='127.0.0.1',
                       user='user',
                       password='password',
                       db='test')

cur = conn.cursor()
cur.execute('select * from dec_test')
cur.fetchall()
d1 = cur.description
cur.close()
conn.close()

# mysqldb

import MySQLdb

conn = MySQLdb.connect(host='127.0.0.1',
                       user='user',
                       passwd='password',
                       db='test')

cur = conn.cursor()
cur.execute('select * from dec_test')
cur.fetchall()
d2 = cur.description
cur.close()
conn.close()

# MySQL Connector/Python

import mysql.connector

conn = mysql.connector.connect(host='127.0.0.1',
                               user='user',
                               password='password',
                               database='test')

cur = conn.cursor()
cur.execute('select * from dec_test')
cur.fetchall()
d3 = cur.description
cur.close()
conn.close()

print(d1)
print(d2)
print(d3)

""" Output:
((u'dec_2_2', 246, None, 6, 6, 2, True),
 (u'dec_4_2', 246, None, 8, 8, 2, True),
 (u'dec_8_4', 246, None, 10, 10, 4, True),
 (u'char_15', 254, None, 15, 15, 0, True))

(('dec_2_2', 246, 5, 6, 6, 2, 1),
 ('dec_4_2', 246, 7, 8, 8, 2, 1),
 ('dec_8_4', 246, 9, 10, 10, 4, 1),
 ('char_15', 254, 11, 45, 45, 0, 1))

[(u'dec_2_2', 246, None, None, None, None, 1, 0),
 (u'dec_4_2', 246, None, None, None, None, 1, 0),
 (u'dec_8_4', 246, None, None, None, None, 1, 0),
 (u'char_15', 254, None, None, None, None, 1, 0)]
"""

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
