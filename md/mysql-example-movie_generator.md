---
title: mysql example movie generator (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'movie generator'


Modules used in program: 
* `import getpass`
* `import mysql.connector`

## python movie generator

Python mysql example: movie generator

```python
#!/usr/bin/env python
#-*- coding:utf-8 -*-

'''
连接数据库，查询表信息，生成markdown文档内容
'''

from __future__ import print_function
import mysql.connector
import getpass

password = getpass.getpass()
conn = mysql.connector \
       .connect(user='root',password=password, \
                       database='luckykoala',use_unicode=True)
cursor = conn.cursor()
cursor.execute('select id, chinese_name, foreign_name from movie')
values = cursor.fetchall()
print('========================')
print('|序号|中文名|外文名|')
print('|:---:|:---:|:---:|')
for t in values:
        '''
        Hardcode for three column table
        '''
        print('|%s|__%s__|_%s_|'% (t[0], t[1], t[2]))
print('========================')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
