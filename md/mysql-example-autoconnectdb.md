---
title: mysql example autoconnectdb (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'autoconnectdb'


Modules used in program: 
* `import time`
* `import _mysql_exceptions`
* `import MySQLdb`

## python autoconnectdb

Python mysql example: autoconnectdb

```python
#!/usr/bin/env python3
import MySQLdb
import _mysql_exceptions
import time


class AutoconnectDB():

	def __init__(self,user,password,db,host=None,unix_socket=None):
		self.__user = user
		self.__password = password
		self.__db = db

	def __connect(self):
		self.__conn = MySQLdb.connect(user=self.__user, passwd=self.__password, db=self.__db, unix_socket="/var/lib/mysql/mysql.sock", connect_timeout=1)
		self.__conn.autocommit(True)

	def doQuery(self,query,input=None,max_retry=5): # tries to do query, auto-reconnect on failure

		tries = 0
		cursor = None

		while tries < max_retry:

			try:
				cursor = self.__conn.cursor(MySQLdb.cursors.DictCursor)
				cursor.execute(query,input)
				break

			except (MySQLdb.OperationalError,AttributeError): # db.OpErr = Last connection was cool and good, but the server disconnected since, Attr = self.__conn is None (not connected yet)

				try:
					self.__connect()

				except _mysql_exceptions.OperationalError: # connection failed
					pass

				if tries > 0: # not the first connection attempt
					time.sleep(0.5)

				tries += 1

		return cursor

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
