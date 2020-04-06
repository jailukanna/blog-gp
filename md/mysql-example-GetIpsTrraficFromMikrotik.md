---
title: mysql example GetIpsTrraficFromMikrotik (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'GetIpsTrraficFromMikrotik'


Modules used in program: 
* `import mysql.connector as mariadb`
* `import routeros_api`
* `import re`

## python GetIpsTrraficFromMikrotik

Python mysql example: GetIpsTrraficFromMikrotik

```python
#! /usr/bin/python3
import re
import routeros_api
import mysql.connector as mariadb

connection = routeros_api.RouterOsApiPool('192.168.1.1', username='username', password='password') 

mariadb_connection = mariadb.connect(user='root', password='password', database='databaseName' , unix_socket="/var/run/mysqld/mysqld.sock")
cursor = mariadb_connection.cursor()

api = connection.get_api()

list = api.get_resource('/command')

print(list)

list_queues = api.get_resource('/queue/simple')

a = list_queues.get()

prog = re.compile("\d{3|2}.168.1.\d{3}/*")

db = cursor.execute("SELECT * FROM PerIpTrafficStat")
db = cursor.fetchall()

########################## just for the first time #######################
#if not db:
#	db = [(0, 0,"192.168.1.%s/32" %ip,0,0,0,0) for ip in range(192, 255)]
##########################################################################

backlog = []
cursor.execute("truncate table PerIpTrafficStat")

for i in a:
	for item in db:
		if (str(i['target']) == str(item[2])):
			upload = int(i['bytes'].split("/")[0]) - item[5] if int(i['bytes'].split("/")[0]) >= item[5] else int(i['bytes'].split("/")[0])
			download = int(i['bytes'].split("/")[1]) - item[6] if int(i['bytes'].split("/")[1]) >= item[6] else int(i['bytes'].split("/")[1])
			last_up = i['bytes'].split("/")[0]
			last_dl = i['bytes'].split("/")[1]

			cursor.execute("INSERT INTO PerIpTrafficStat (ip, target_ip, upload, download, last_up, last_dl) VALUES (%s, %s, %s, %s, %s, %s)",
					(i['name'], i['target'], upload, download, last_up, last_dl))


mariadb_connection.commit()
mariadb_connection.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
