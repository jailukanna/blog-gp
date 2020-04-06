---
title: mysql example rideable outages to sql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'rideable outages to sql'


Modules used in program: 
* `import sys`
* `import mysql.connector`
* `import feedparser`
* `import pandas as pd`
* `import json`
* `import requests`

## python rideable outages to sql

Python mysql example: rideable outages to sql

```python
#!/usr/bin/env python
# coding: utf-8



import requests
import json
from pandas.io.json import json_normalize
import pandas as pd
import feedparser
import mysql.connector
import sys


station_id_input = sys.argv[1]
station_score_input = sys.argv[2]

######Opens database connection

server = #server host
db = #db name    #'rideable'
username = #username
pw = #password
cnx = mysql.connector.connect(user=username, password=pw,
                              host=server,
                              database=db)

cursor = cnx.cursor()     # get the cursor

tables = cursor.execute("show tables")    # execute 'SHOW TABLES' (but data is not returned

for (table_name,) in cursor:
        print(table_name)

#####Creates necessary db tables 

sql_subway_raw_drop = "drop table if exists rideable.subway_live_update_raw;"
sql_subway_raw_create = "create table rideable.subway_live_update_raw (arrival_time varchar(255),departure_time varchar(255),schedule_relationship varchar(255),stop_id varchar(10),ind_train_line varchar(10),trip_id varchar(255),current_status varchar(255),trains_id varchar(255),timestamp_updated varchar(255));"

sql_elevators_raw_drop = "drop table if exists rideable.elevator_escalator_status_raw;"
sql_elevators_raw_create = "create table rideable.elevator_escalator_status_raw (accessible_note varchar(255),elevator_count_words varchar(255),escalator_count_words varchar(255),has_machines varchar(255),station_id int,is_accessible varchar(255),subway_lines varchar(255),station_name varchar(255),outages varchar(255));"

cursor.execute(sql_subway_raw_drop)
cursor.execute(sql_subway_raw_create)
cursor.execute(sql_elevators_raw_drop)
cursor.execute(sql_elevators_raw_create) 
#cnx.commit()



###Subway raw feed
data = json.load(open('feed.json','r'))


live_df = pd.DataFrame()


rows = []
for num in range(int(len(data['entity'])/2)):
    if num % 2 == 0:
        rows.append(num)



for i in rows:
    j=i+1
    temp = pd.DataFrame.from_dict(json_normalize(data['entity'][i]['tripUpdate']['stopTimeUpdate']), orient='columns')
    temp['ind_train_line'] = data['entity'][i]['tripUpdate'] ['trip']['routeId']
    temp['trip_id'] = data['entity'][i]['tripUpdate']['trip']['tripId']
    temp['current_status'] = data['entity'][j]['vehicle']['currentStatus']
    temp['trains_id'] = data['entity'][i]['id']
    temp['timestamp_updated'] = data['header']['timestamp']
    live_df = pd.concat([live_df, temp], ignore_index=True)
    



insert_stmt = "insert into rideable.subway_live_update_raw values (%s,%s,%s,%s,%s,%s,%s,%s,%s)"

by_row = []
for i in range(len(live_df)):
    row = ((str(live_df['arrival.time'][i])
                , str(live_df['departure.time'][i])
                , str(live_df['scheduleRelationship'][i])
                , str(live_df['stopId'][i])
                , str(live_df['ind_train_line'][i])
                , str(live_df['trip_id'][i])
                , str(live_df['current_status'][i])
                , str(live_df['trains_id'][i])
                , str(live_df['timestamp_updated'][i])))
    by_row.append(row)



cursor.executemany(insert_stmt,by_row)

#cnx.commit()




#Elevator/escalator outages
#API from NYC Accessible
elevators = requests.get("http://www.nycaccessible.com/api/v1/stations/").json()
elevs = pd.DataFrame.from_dict(json_normalize(elevators),orient='columns')


elv_query = 'insert into rideable.elevator_escalator_status_raw values (%s,%s,%s,%s,%s,%s,%s,%s,%s)'

els = []

for e in range(len(elevs)):
    e_row = (str(elevs['accessible_note'][e])
        ,str(elevs['elevator_count_words'][e])
        ,str(elevs['escalator_count_words'][e])
        ,str(elevs['has_machines'][e])
        ,str(elevs['id'][e])
        ,str(elevs['is_accessible'][e])
        ,str(elevs['lines'][e])
        ,str(elevs['name'][e])
        ,str(elevs['outages'][e]))
    els.append(e_row)


cursor.executemany(elv_query,els)


#cnx.commit()


##More SQL tables

sql_elevator_lookup_drop = "drop table if exists rideable.elevator_escalator_status_lookup_table;"
sql_elevator_lookup_create = "create table rideable.elevator_escalator_status_lookup_table (station_id varchar(255),station_name varchar(255),num_elevators varchar(255),num_escalators varchar(255),outages varchar(255));"
sql_elevator_lookup_insert = "insert into rideable.elevator_escalator_status_lookup_table (select station_id,station_name,case when trim(left(elevator_count_words,2)) not like 'No' and trim(left(elevator_count_words,2)) not like '' then trim(left(elevator_count_words,2)) else 0 end as num_elevators,case when trim(left(escalator_count_words,2)) not like 'No' and trim(left(escalator_count_words,2)) not like '' then trim(left(escalator_count_words,2)) else 0 end as num_escalators,case when trim(left(outages,2)) not like 'No' and trim(left(outages,2)) not like '' then trim(left(outages,2)) else null end as outages from rideable.elevator_escalator_status_raw where accessible_note not like 'None' group by 1,2,3,4,5);"

cursor.execute(sql_elevator_lookup_drop)
cursor.execute(sql_elevator_lookup_create)
cursor.execute(sql_elevator_lookup_insert)

#cnx.commit()

sql_station_outages_drop = "drop table if exists rideable.station_outages;"
sql_station_outages_create = "create table rideable.station_outages (station_id int,station_stop_id varchar(255),stop_name varchar(255),latitude float,longitude float,train_line_1 int,train_line_2 int,train_line_3 int,train_line_4 int,train_line_5 int,train_line_6 int,train_line_7 int,train_line_A int,train_line_C int,train_line_E int,train_line_N int,train_line_Q int,train_line_R int ,train_line_W int,train_line_B int,train_line_D int,train_line_F int,train_line_M int,train_line_J int,train_line_Z int,train_line_L int,train_line_G int,train_line_SIR int,train_line_S int,typically_accessible int,num_elevators int,num_escalators int,wheelchair_accessible_problem int,visually_impaired_problem int,accessibility_score int);"
sql_station_outages_insert = "insert into rideable.station_outages (select c.*,case when c.station_id="+station_id_input+" then "+station_score_input+" when c.typically_accessible=1 and c.wheelchair_accessible_problem=0 and c.visually_impaired_problem=0 then 0 when c.typically_accessible=0 then 1 when c.typically_accessible=1 and c.wheelchair_accessible_problem=0 and c.visually_impaired_problem>0 then 2 when c.wheelchair_accessible_problem=1 and c.visually_impaired_problem>0 then 3  else 0 end as accessibility_score from (select a.station_id,a.station_stop_id,a.stop_name,a.latitude,a.longitude,a.train_line_1,a.train_line_2,a.train_line_3,a.train_line_4,a.train_line_5,a.train_line_6,a.train_line_7,a.train_line_A,a.train_line_C,a.train_line_E,a.train_line_N,a.train_line_Q,a.train_line_R ,a.train_line_W,a.train_line_B,a.train_line_D,a.train_line_F,a.train_line_M,a.train_line_J,a.train_line_Z,a.train_line_L,a.train_line_G ,a.train_line_SIR ,a.train_line_S ,case when a.ada_accessible is null then 0 else a.ada_accessible end as typically_accessible ,cast(b.num_elevators as unsigned) as num_elevators ,cast(b.num_escalators as unsigned) as num_escalators ,case when cast(b.outages as unsigned)>0 then 1 else 0 end as wheelchair_accessible_problem,0 as visually_impaired_problem from rideable.stations_lookup_table a left join rideable.elevator_escalator_status_lookup_table b on cast(a.station_id as char) = b.station_id group by 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34 order by 1 ) c group by 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35);"

#when c.station_id= 25 or c.station_id= 174 then 0

cursor.execute(sql_station_outages_drop)
cursor.execute(sql_station_outages_create)
cursor.execute(sql_station_outages_insert)

cnx.commit()

cursor.close()







```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
