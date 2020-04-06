---
title: mysql example untitled Mysql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'untitled Mysql'


Modules used in program: 
* `import pymysql`

## python untitled Mysql

Python mysql example: untitled Mysql

```python
import pymysql
class Mysql(object):
    def __init__(self):
        try:
            self.conn = pymysql.connect(
                host='192.168.137.3',
                port=3306,
                user='root',
                passwd='wangrui103',
                db='db1',
                charset='utf8'
            )
        except Exception as e:
            print(e)
        else:
            print('连接成功')
            self.cur = self.conn.cursor()

    def create_table(self):
        sql = 'create table testtb(id int, name varchar(10),age int)'
        res = self.cur.execute(sql)
        print(res)

    def close(self):
        self.cur.close()
        self.conn.close()

    def add(self,sql):  # 增
        # sql = 'insert into testtb values(1,"Tom",18),(2,"Jerry",16),(3,"Hank",24)'
        res = self.cur.execute(sql)
        if res:
            self.conn.commit()
        else:
            self.conn.rollback()
        print(res)

    def rem(self):  # 删
        sql = 'delete from testtb where id=1'
        res = self.cur.execute(sql)
        if res:
            self.conn.commit()
        else:
            self.conn.rollback()
        print(res)

    def mod(self):  # 改
        sql = 'update testtb set name="Tom Ding" where id=2'
        res = self.cur.execute(sql)
        if res:
            self.conn.commit()
        else:
            self.conn.rollback()
        print(res)

    def show(self,sql):  # 查
        # sql = 'select * from testtb'
        self.cur.execute(sql)
        res = self.cur.fetchall()
        return res



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
