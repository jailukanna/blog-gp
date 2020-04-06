---
title: mysql example BarTiming (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'BarTiming'

Functions in program: 
* `def executeQuery(conn , index):`

Modules used in program: 
* `import mysql.connector`

## python BarTiming

Python mysql example: BarTiming

```python
import mysql.connector

host = "INSERT_HOST"
username = "ROOT_USER"
database = "MY_DB"
password = "INSER_PASS"

paramArray = [
  # Sunday 12 - 15
  "2019-05-26 00:20:07",
  "2019-05-26 01:20:07",
  "2019-05-26 02:20:07",
  "2019-05-26 03:20:07",
  "2019-05-26 04:20:07",
  "2019-05-26 05:20:07",
  "2019-05-26 06:20:07",
  "2019-05-26 07:20:07",
  "2019-05-26 08:03:07",
  "2019-05-26 09:20:07",
  "2019-05-26 10:20:07",
  "2019-05-26 11:20:07",
  "2019-05-26 12:20:07",
  "2019-05-26 13:20:07",
  "2019-05-26 14:20:07",
  "2019-05-26 15:20:07",
  "2019-05-26 16:20:07",
  "2019-05-26 17:20:07",
  "2019-05-26 18:03:07",
  "2019-05-26 19:20:07",
  "2019-05-26 20:20:07",
  "2019-05-26 21:20:07",
  "2019-05-26 22:20:07",
  "2019-05-26 23:20:07",
  # Monday 06 - 05T
  "2019-05-27 00:20:07",
  "2019-05-27 01:20:07",
  "2019-05-27 02:20:07",
  "2019-05-27 03:20:07",
  "2019-05-27 04:20:07",
  "2019-05-27 05:20:07",
  "2019-05-27 06:20:07",
  "2019-05-27 07:20:07",
  "2019-05-27 08:03:07",
  "2019-05-27 09:20:07",
  "2019-05-27 10:20:07",
  "2019-05-27 11:20:07",
  "2019-05-27 12:20:07",
  "2019-05-27 13:20:07",
  "2019-05-27 14:20:07",
  "2019-05-27 15:20:07",
  "2019-05-27 16:20:07",
  "2019-05-27 17:20:07",
  "2019-05-27 18:03:07",
  "2019-05-27 19:20:07",
  "2019-05-27 20:20:07",
  "2019-05-27 21:20:07",
  "2019-05-27 22:20:07",
  "2019-05-27 23:20:07",
  # Tuesday 06 - 14
  "2019-05-28 00:20:07",
  "2019-05-28 01:20:07",
  "2019-05-28 02:20:07",
  "2019-05-28 03:20:07",
  "2019-05-28 04:20:07",
  "2019-05-28 05:20:07",
  "2019-05-28 06:20:07",
  "2019-05-28 07:20:07",
  "2019-05-28 08:03:07",
  "2019-05-28 09:20:07",
  "2019-05-28 10:20:07",
  "2019-05-28 11:20:07",
  "2019-05-28 12:20:07",
  "2019-05-28 13:20:07",
  "2019-05-28 14:20:07",
  "2019-05-28 15:20:07",
  "2019-05-28 16:20:07",
  "2019-05-28 17:20:07",
  "2019-05-28 18:03:07",
  "2019-05-28 19:20:07",
  "2019-05-28 20:20:07",
  "2019-05-28 21:20:07",
  "2019-05-28 22:20:07",
  "2019-05-28 23:20:07",
  # Wednesday 06 - 12
  "2019-05-29 00:20:07",
  "2019-05-29 01:20:07",
  "2019-05-29 02:20:07",
  "2019-05-29 03:20:07",
  "2019-05-29 04:20:07",
  "2019-05-29 05:20:07",
  "2019-05-29 06:20:07",
  "2019-05-29 07:20:07",
  "2019-05-29 08:03:07",
  "2019-05-29 09:20:07",
  "2019-05-29 10:20:07",
  "2019-05-29 11:20:07",
  "2019-05-29 12:20:07",
  "2019-05-29 13:20:07",
  "2019-05-29 14:20:07",
  "2019-05-29 15:20:07",
  "2019-05-29 16:20:07",
  "2019-05-29 17:20:07",
  "2019-05-29 18:03:07",
  "2019-05-29 19:20:07",
  "2019-05-29 20:20:07",
  "2019-05-29 21:20:07",
  "2019-05-29 22:20:07",
  "2019-05-29 23:20:07",
  # Thursday 06 - 13
  "2019-05-30 00:20:07",
  "2019-05-30 01:20:07",
  "2019-05-30 02:20:07",
  "2019-05-30 03:20:07",
  "2019-05-30 04:20:07",
  "2019-05-30 05:20:07",
  "2019-05-30 06:20:07",
  "2019-05-30 07:20:07",
  "2019-05-30 08:03:07",
  "2019-05-30 09:20:07",
  "2019-05-30 10:20:07",
  "2019-05-30 11:20:07",
  "2019-05-30 12:20:07",
  "2019-05-30 13:20:07",
  "2019-05-30 14:20:07",
  "2019-05-30 15:20:07",
  "2019-05-30 16:20:07",
  "2019-05-30 17:20:07",
  "2019-05-30 18:03:07",
  "2019-05-30 19:20:07",
  "2019-05-30 20:20:07",
  "2019-05-30 21:20:07",
  "2019-05-30 22:20:07",
  "2019-05-30 23:20:07",
  # Firday 06 - 11
  "2019-05-31 00:20:07",
  "2019-05-31 01:20:07",
  "2019-05-31 02:20:07",
  "2019-05-31 03:20:07",
  "2019-05-31 04:20:07",
  "2019-05-31 05:20:07",
  "2019-05-31 06:20:07",
  "2019-05-31 07:20:07",
  "2019-05-31 08:03:07",
  "2019-05-31 09:20:07",
  "2019-05-31 10:20:07",
  "2019-05-31 11:20:07",
  "2019-05-31 12:20:07",
  "2019-05-31 13:20:07",
  "2019-05-31 14:20:07",
  "2019-05-31 15:20:07",
  "2019-05-31 16:20:07",
  "2019-05-31 17:20:07",
  "2019-05-31 18:03:07",
  "2019-05-31 19:20:07",
  "2019-05-31 20:20:07",
  "2019-05-31 21:20:07",
  "2019-05-31 22:20:07",
  "2019-05-31 23:20:07",
  # Saturday 11 - 15
  "2019-06-01 00:20:07",
  "2019-06-01 01:20:07",
  "2019-06-01 02:20:07",
  "2019-06-01 03:20:07",
  "2019-06-01 04:20:07",
  "2019-06-01 05:20:07",
  "2019-06-01 06:20:07",
  "2019-06-01 07:20:07",
  "2019-06-01 08:03:07",
  "2019-06-01 09:20:07",
  "2019-06-01 10:20:07",
  "2019-06-01 11:20:07",
  "2019-06-01 12:20:07",
  "2019-06-01 13:20:07",
  "2019-06-01 14:20:07",
  "2019-06-01 15:20:07",
  "2019-06-01 16:20:07",
  "2019-06-01 17:20:07",
  "2019-06-01 18:03:07",
  "2019-06-01 19:20:07",
  "2019-06-01 20:20:07",
  "2019-06-01 21:20:07",
  "2019-06-01 22:20:07",
  "2019-06-01 23:20:07"
]

paramResultArray = [
  # Sunday 12 - 15
0,0,0,0,0,0,0,0,0,0,0,0,1,1,1,0,0,0,0,0,0,0,0,0,
  # Monday 06 - 05T
0,0,0,0,0,0,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,
  # Tuesday 06 - 14
1,1,1,1,1,0,1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0,
  # Wednesday 06 - 12
0,0,0,0,0,0,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0,0,0,
  # Thursday 06 - 13
0,0,0,0,0,0,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0,0,
  # Firday 06 - 11
0,0,0,0,0,0,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0,0,0,0, 
 # Saturday 11 - 15
0,0,0,0,0,0,0,0,0,0,0,1,1,1,1,0,0,0,0,0,0,0,0,0]

def executeQuery(conn , index):
  param = paramArray[index]
  sql_select_Query = """
select * FROM (select (%s BETWEEN t1.openingTimeDate AND t1.closingTimeDate) as isOpened , 
(CASE WHEN (%s BETWEEN t1.openingTimeDate AND t1.closingTimeDate) = 1 THEN TIMESTAMPDIFF(MINUTE,%s,t1.closingTimeDate) 
ELSE 0 END) as closingInMinutes from 
(select *, 
DAYNAME( DATE_ADD( concat( STR_TO_DATE( CONCAT('2019 2 ',day), '%X %V %W') ,' ', `opening_time`) ,
 INTERVAL `calculated_time` SECOND) ) as closingDay , (CASE WHEN DAYNAME(%s) = day THEN
   concat(Date(%s),' ', `opening_time`) ELSE concat( DATE_SUB( Date(%s) , INTERVAL 1 DAY ),' ', `opening_time`)  END )
     as openingTimeDate, DATE_ADD(CASE WHEN DAYNAME(%s) = day THEN  concat(Date(%s),' ', `opening_time`) 
     ELSE concat( DATE_SUB( Date(%s) , INTERVAL 1 DAY ),' ', `opening_time`) END, INTERVAL `calculated_time` SECOND ) 
     as closingTimeDate from `establishment_timings` where `establishment_id` = 159 ) as t1 where DAYNAME(%s) = t1.day 
     OR DAYNAME(%s) = t1.closingDay) as t2 where t2.isOpened = 1
   """
  # sql_select_Query = "SELECT * FROM `establishment_timings`"
  cursor = conn.cursor()
  cursor.execute(sql_select_Query , (param,param,param,param,param,param,param,param,param,param,param))
  records = cursor.fetchall()
  cursor.close()
  if len(records) == 0:
    print(str(index)+" -> "+param+" isOpened 0 -- expected "+str(paramResultArray[index]))
  for row in records:
    print(str(index)+" -> "+param+" isOpened "+str(row[0])+ " -- expected " +str(paramResultArray[index]))
  
  index = index+1
  if index == len(paramArray):
    print("break")
    return
  else:
    executeQuery(conn , index)



try:
  print("Start Connection")
  mySQLconnection = mysql.connector.connect(
  host= host,
  user= username,
  passwd= password,
  database = database
)
  print("Connecting Complete")
  executeQuery(mySQLconnection , 0)
   
except Exception as e:
  print(("Error MySQL", e))
finally:
  #closing database connection.
  if(mySQLconnection .is_connected()):
    mySQLconnection.close()
    print("MySQL connection is closed")


mockData = """
INSERT INTO `establishment_timings` (`id`, `establishment_id`, `day`, `opening_time`, `closed_time`, `calculated_time`, `status`, `created_at`, `updated_at`)
VALUES
	(916, 159, 'Monday', '06:00:00', '05:00:00', '82800', 'open', '2019-05-23 09:35:13', '2019-05-29 07:07:12'),
	(917, 159, 'Tuesday', '06:00:00', '14:00:00', '28800', 'closed', '2019-05-23 09:35:13', '2019-05-29 07:07:12'),
	(918, 159, 'Wednesday', '06:00:00', '12:00:00', '21600', 'open', '2019-05-23 09:35:13', '2019-05-29 07:07:12'),
	(919, 159, 'Thursday', '06:00:00', '13:00:00', '25200', 'open', '2019-05-23 09:35:13', '2019-05-29 07:07:12'),
	(920, 159, 'Friday', '06:00:00', '11:00:00', '18000', 'open', '2019-05-23 09:35:13', '2019-05-29 07:07:12'),
	(921, 159, 'Saturday', '11:00:00', '15:00:00', '14400', 'closed', '2019-05-23 09:35:13', '2019-05-29 07:07:12'),
	(922, 159, 'Sunday', '12:00:00', '15:00:00', '10800', 'open', '2019-05-23 09:35:13', '2019-05-29 07:07:12');
"""

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com