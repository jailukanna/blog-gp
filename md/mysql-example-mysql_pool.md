---
title: mysql example mysql pool (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql pool'

Functions in program: 
* `def enable(pool_size, recycle):`

Modules used in program: 
* `import MySQLdb`
* `import Queue`
* `import logging`
* `import threading`
* `import time`

## python mysql pool

Python mysql example: mysql pool

```python
# coding: utf-8
"inspired by sqlalchemy/pool.py"
import time
import threading
import logging
import Queue
import MySQLdb
from MySQLdb import connections, Error
from MySQLdb.connections import Connection


class QueuePool:
    def __init__(self, creator, pool_size=5, recycle=None):
        """
        :param creator: 回调函数, 返回值为连接对象
        :param pool_size: 连接池大小, 最多保持几个连接
        :param recycle: 连接保持时间(秒), 不能超过mysql的wait_timeout.
                        查看wait_timeout的方法: show variables like 'wait_timeout'
        """
        self.creator = creator
        self.recycle = recycle
        self.q = Queue.Queue(pool_size)
        self.cset = set()  # 保证队列成员不重复

    def create_connection(self):
        c = self.creator()
        c._lock = threading.Lock()
        c._pool = self
        c._activetime = time.time()
        return c

    def close(self, conn):
        if not conn.open:
            return
        del conn._pool
        conn._close()

    def connect(self):
        try:
            while 1:
                c = self.q.get(False) # noblock
                if c in self.cset:
                    self.cset.remove(c)
                now = time.time()
                if self.recycle is not None and now - c._activetime >= self.recycle:
                    self.close(c)
                else:
                    #XXX: 以连接时间为起点还是以每次查询时间为起点?
                    c._activetime = now
                    #XXX: 此时连接是否还活着? 如果在排队期间断线了, 取出来查询的时候就会报
                    #(2006, MySQL server has gone away), 报错说明数据库确实出问题了.
                    return c
        except Queue.Empty:
            return self.create_connection()

    def return_conn(self, conn):
        if not conn.open:
            return
        if conn in self.cset:
            return
        if time.time() - conn._activetime >= self.recycle:
            self.close(conn)
            return
        try:
            self.cset.add(conn)
            self.q.put(conn, False)
        except Queue.Full:
            logging.warning('QueuePool Full: %s' % self.q.qsize())
            self.close(conn)

    def size(self):
        return self.q.maxsize

    def len(self):
        return self.q.qsize()

    def clear(self):
        while 1:
            try:
                c = self.q.get(False)
                self.close(c)
            except Queue.Empty:
                break


raw_connect = MySQLdb.connect

class PoolManager:

    def __init__(self, pool_size=8, recycle=30):
        self.pool_size = pool_size
        self.recycle = recycle
        self._pools = {}
        self._get_pool_lock = threading.Lock()

    def connect(self, **kw):
        def creator():
            c = raw_connect(**kw)
            return c

        with self._get_pool_lock:
            key = (kw['host'], kw['port'], kw['user'], kw['db'])
            pool = self._pools.get(key)
            if not pool:
                pool = self._pools[key] = QueuePool(creator, pool_size=self.pool_size, recycle=self.recycle)
        return pool.connect()

    def close_connection(self, conn):
        if hasattr(conn, '_pool'):
            conn._pool.return_conn(conn)

    def conn_query(self, conn, sql):
        try:
            return conn._query(sql)
        except Error as e:
            # destroy the connection when connection break
            if e[0] in (2006, 2013):
                conn._pool.close(conn)
            raise


def enable(pool_size, recycle):
    """enable connection pool"""
    manager = PoolManager(pool_size, recycle)

    def im_connect(**kw):
        return manager.connect(**kw)

    def im_close(self):
        return manager.close_connection(self)

    def im_query(self, sql):
        return manager.conn_query(self, sql)

    MySQLdb.connect = im_connect
    Connection._close = Connection.close
    Connection.close = im_close  # 此时close不再关闭连接, 而是归还连接池, 查询完应该主动调用归还连接
    Connection._query = Connection.query
    Connection.query = im_query


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
