---
title: mysql example rideable get mta feed (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'rideable get mta feed'


Modules used in program: 
* `import os`
* `import json`
* `import sys`
* `import requests`

## python rideable get mta feed

Python mysql example: rideable get mta feed

```python
#!/usr/bin/env python
# coding: utf-8



from google.transit import gtfs_realtime_pb2
import requests
import sys
from google.protobuf import json_format
import json
import os

api_key = #[get api key from MTA]

url1 = ' http://datamine.mta.info/mta_esi.php?key=091bf556bd6e225ab1a18d71e8f162ee&feed_id=1'
url2 = ' http://datamine.mta.info/mta_esi.php?key=091bf556bd6e225ab1a18d71e8f162ee&feed_id=26'
url3 = ' http://datamine.mta.info/mta_esi.php?key=091bf556bd6e225ab1a18d71e8f162ee&feed_id=16'
url4 = ' http://datamine.mta.info/mta_esi.php?key=091bf556bd6e225ab1a18d71e8f162ee&feed_id=21'
url5 = ' http://datamine.mta.info/mta_esi.php?key=091bf556bd6e225ab1a18d71e8f162ee&feed_id=2'
url6 = ' http://datamine.mta.info/mta_esi.php?key=091bf556bd6e225ab1a18d71e8f162ee&feed_id=11'
url7 = ' http://datamine.mta.info/mta_esi.php?key=091bf556bd6e225ab1a18d71e8f162ee&feed_id=31'
url8 = ' http://datamine.mta.info/mta_esi.php?key=091bf556bd6e225ab1a18d71e8f162ee&feed_id=36'
url9 = ' http://datamine.mta.info/mta_esi.php?key=091bf556bd6e225ab1a18d71e8f162ee&feed_id=51'


##usage python getMtaLiveJson.py <route number>
##FS =  Franklin Ave shuttle

routenum = all
#sys.argv[1]
#Can switch to routenum=sys.argv[1] to look up a specific subway line

if routenum == 'all':
    url = 'http://datamine.mta.info/mta_esi.php?key='+api_key

if routenum == '1' or routenum == '2' or routenum == '3' or routenum == '4' or routenum == '5' or routenum == '6' or routenum == 'S':
    url = url1

if routenum == 'A' or routenum == 'C' or routenum == 'E' or routenum == 'H' or routenum == 'FS':
    url = url2

if routenum == 'N' or routenum == 'Q' or routenum == 'R' or routenum == 'W':
    url = url3

if routenum == 'B' or routenum == 'D' or routenum == 'F' or routenum == 'M':
    url = url4

if routenum == 'L':
    url = url5

if routenum == 'SIR':
    url = url6

if routenum == 'G':
    url = url7

if routenum == 'J' or 'Z':
    url = url8

if routenum == '7':
    url = url9


feed = gtfs_realtime_pb2.FeedMessage()
response = requests.get(url)
feed.ParseFromString(response.content)

##
##for entity in feed.entity:
##  if entity.HasField('trip_update'):
##    print((entity.trip_update))

jsonout = json_format.MessageToJson(feed)

with open("feed.json","w") as f:
    f.write(jsonout)



data = json.load(open('feed.json','r'))


station_id_input = sys.argv[1]
station_score_input = sys.argv[2]
#Accessibility scores: 0=no issues, 1=wheelchair access issue, 2=visual impairment issue, 3=wheelchair and visual impairment issue


folder = [insert filepath]

script_to_run = folder + "rideable_output_to_sql.py "

command = 'python '+script_to_run + station_id_input +" "+station_score_input
os.system(command)
#Triggers Python2 script. We couldn't connect to the database using Python3.


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
