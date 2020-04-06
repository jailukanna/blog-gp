---
title: mysql example dbdoc (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'dbdoc'

Functions in program: 
* `def main():`
* `def print_table(table):`
* `def filter_exclude(table_name):`
* `def execute(statement, *args, **kwargs):`

Modules used in program: 
* `import sys`
* `import fnmatch`
* `import argparse`

## python dbdoc

Python mysql example: dbdoc

```python
#!/usr/bin/env python3
import argparse
import fnmatch
import sys
from sqlalchemy import create_engine

# 检测 MySQLdb 或 mysql.connector 是否存在
try:
    import MySQLdb
except ImportError:
    try:
        import mysql.connector
    except ImportError:
        print('MySQLdb and mysql.connector not found.')
        sys.exit(1)
    else:
        mysql_url_template = 'mysql+mysqlconnector://{}?charset=utf8'
else:
    mysql_url_template = 'mysql://{}?charset=utf8'


# 模板
DOC_HEADER = '''\
# {dbname}

[[_TOC_]]
'''


TABLE_INFO_TEMPLATE = '''\
## {Name}

{Comment}

'''

COLUMN_HEADER = '''\
| Field | Type | Null | Key | Default | Comment |
| ---- | ---- | ---- | ---- | ---- | ---- |
'''


def execute(statement, *args, **kwargs):
    """封装 engine.execute"""
    try:
        result = engine.execute(statement, *args, **kwargs)
    except Exception as e:
        print('Error on execute %r: ' % statement, e)
        sys.exit(1)
    return result


def filter_exclude(table_name):
    """表名过滤"""
    for exclude in args.exclude:
        if fnmatch.fnmatch(table_name, exclude):
            return True
    return False


def print_table(table):
    """输出表相关信息"""
    if filter_exclude(table.Name):
        return

    print(TABLE_INFO_TEMPLATE.format_map(table), end='')

    print(COLUMN_HEADER, end='')

    for column in execute('SHOW FULL COLUMNS FROM ' + table.Name):
        column = dict(column)
        column['Comment'] = column['Comment'].replace('\r', '').replace('\n', '<br>')
        print('| {Field} | {Type} | {Null} | {Key} | {Default} | {Comment} |'.format_map(column))

    print()


def main():
    # 分析命令行参数
    parser = argparse.ArgumentParser()

    parser.add_argument('-u', '--url', default='root@localhost/test', help='Database URI.')
    parser.add_argument('-x', '--exclude', default='', help='Exclude tables(support *, ?).')

    global args
    args = parser.parse_args()
    args.exclude = args.exclude.split(';')

    # 连接数据库
    global engine
    engine = create_engine(mysql_url_template.format(args.url))

    # 当前数据库名
    dbname = list(execute('SELECT DATABASE()'))[0][0]

    print(DOC_HEADER.format(dbname=dbname))

    for table in execute('SHOW TABLE STATUS'):
        print_table(table)


if __name__ == "__main__":
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
