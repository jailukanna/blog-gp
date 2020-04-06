---
title: mysql example 3 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example '3'

Functions in program: 
* `def insertPythonVaribleInTable(ticket_number, start_, finish, start_totalizer, end_totalizer, start_count, end_count, gross_deliver, avg_flowrate, sale_number):`

Modules used in program: 
* `import csv`
* `import time`
* `import re`

## python 3

Python mysql example: 3

```python
# import serial
import re
import time
import csv
# import mysql.connector
# from mysql.connector import Error
# from mysql.connector import errorcode
from datetime import datetime

from pprint(import pprint)

# """
def insertPythonVaribleInTable(ticket_number, start_, finish, start_totalizer, end_totalizer, start_count, end_count, gross_deliver, avg_flowrate, sale_number):
	try:
		connection = mysql.connector.connect(host='localhost',
											 database='mydatabase1',
											 user='root',
											 password='password')
		cursor = connection.cursor()
		sql_insert_query = """ INSERT INTO `ticket`
								(`ticket_number`, `start_`, `finish`, `start_totalizer`, `end_totalizer`, `start_count`, `end_count`, `gross_deliver`, `avg_flowrate`, `sale_number`) VALUES (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)"""
		insert_tuple = (ticket_number, start_, finish, start_totalizer, end_totalizer,
						start_count, end_count, gross_deliver, avg_flowrate, sale_number)
		cursor.execute(sql_insert_query, insert_tuple)
		connection.commit()
		print("Record inserted successfully into ticket table")
	except mysql.connector.Error as error:
		connection.rollback()  # rollback if any exception occured
		print("Failed inserting record into python_users table {}".format(error))
	finally:
		# closing database connection.
		if(connection.is_connected()):
			cursor.close()
			connection.close()
			print("MySQL connection is closed")




ser = serial.Serial(
	port='COM4',
	baudrate=9600,
	parity=serial.PARITY_NONE,
	stopbits=serial.STOPBITS_ONE,
	bytesize=serial.EIGHTBITS,
	timeout=100)
print("connected to: " + ser.portstr)
# """

regex = r"(?P<ticket_number>TICKET NUMBER.+?$)|(?P<start_>START\s+\d.+)|(?P<finish>FINISH\s+\d.+)|(?P<start_totalizer>START TOTALIZER\s+\d+)|(?P<end_totalizer>END TOTALIZER\s+\d+)|(?P<start_count>START COUNT\s+(\d|\.)+)|(?P<end_count>END COUNT\s+(\d|\.)+)|(?P<gross_deliver>GROSS DELIVER\s+(\d|\.)+)|(?P<avg_flowrate>AVG FLOW RATE\s+(\d|\.)+)|(?P<sale_number>SALE NUMBER\s+(\d|\.)+)|(?P<duplicate_ticket>DUPLICATE TICKET\s+(\d|\.)+)"
data = []

while True:
	# test_str = ser.readline().decode('utf-8')
	test_str = """?v╔?v╔?% ?2?R ?U ?c46?{ ?! ?!╔?v╔TICKET NUMBER				   480



?v╔

?v╔START			 15/01/19 14:54:35

?v╔FINISH			15/01/19 14:54:47

?v╔START TOTALIZER				5578"""
	# ser.flushInput()
	matches = re.finditer(regex, test_str, re.MULTILINE)
	
	for matchNum, match in enumerate(matches, start=1):
		gabung = ("{match}".format(matchNum=matchNum,
								   start=match.start(), end=match.end(), match=match.group()))

		for groupNum in range(0, len(match.groups())):
			groupNum = groupNum + 1
		pattern = '\d+?[\d\.\/\s\:]*?$'
		result = re.findall(pattern, gabung)
		print(result)
		data.append(result[0])
		continue
	print(data)
	hasil = ", ".join(data)
	hasil=hasil.replace('\r','')
	print(hasil)
	# """
	insertPythonVaribleInTable(data['ticket_number'], data['start_'], data['finish'], data['start_totalizer'], data['end_totalizer'],
							   data['start_count'], data['end_count'], data['gross_deliver'], data['avg_flowrate'], data['sale_number'])
	# """
	
	break

# hasil = ", ".join(data).replace('\r','')
# print(hasil)
# ser.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
