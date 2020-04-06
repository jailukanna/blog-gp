---
title: mysql example simple socket db util config (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'simple socket db util config'


Modules used in program: 
* `import MySQLdb  # creator`

## python simple socket db util config

Python mysql example: simple socket db util config

```python
# coding:utf-8
import MySQLdb  # creator
# 返回dict类型数据,tuple类型数据,基本数据类型
from MySQLdb.cursors import DictCursor, Cursor

DB_HOST = "localhost"   #
DB_PORT = 3306          #
DB_USER = "root"        #
DB_PWD = "ziqiancc"     #
DB_NAME = "vehicle"     #
DB_CHAR = "utf8"        #
DB_Creator = MySQLdb    #
DB_minCached = 1        # 启动时开启的空连接数量
DB_maxCached = 20       # 连接池最大可用连接数量
DB_UseUnicode = False   #
DB_Cursor = DictCursor      #

# dbapi ：数据库接口
# maxshared ：连接池最大可共享连接数量
# maxconnections ：最大允许连接数量
# blocking ：达到最大数量时是否阻塞
# maxusage ：单个连接最大复用次数


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
