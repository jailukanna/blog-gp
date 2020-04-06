---
title: mysql example code-import (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'code-import'


Modules used in program: 
* `import sys`
* `import time`
* `import mysql.connector`
* `import dateutil.parser`
* `import requests`
* `import re`

## python code-import

Python mysql example: code-import

```python
import re
import requests
import dateutil.parser
from datetime import datetime
import mysql.connector
import time
import sys

regex_data = r"<BTMessage((?!<\/BTMessage>).)*<\/BTMessage>"
regex_value = r"<([^>]*)>([^<]*)<\/[^>]*>"

url = "https://www.bluetraker.net/ExternalServiceWS/ExternalService.asmx"

payload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\n<soap12:Envelope xmlns:xsi=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns:xsd=\"http://www.w3.org/2001/XMLSchema\" xmlns:soap12=\"http://www.w3.org/2003/05/soap-envelope\">\n    <soap12:Header>\n        <AuthHeader xmlns=\"https://tds.ema.si/\">\t\n            <Username>elcom</Username>\n            <Password>FAz*4*hp</Password>\n        </AuthHeader>\n    </soap12:Header>\n    <soap12:Body>\n        <GetParsedMessages xmlns=\"https://tds.ema.si/\">\n            <fromID>97864577</fromID>\n            <count>999</count>\n        </GetParsedMessages>\n    </soap12:Body>\n</soap12:Envelope>"
headers = {
    'Content-Type': "text/xml",
    'cache-control': "no-cache",
    'Postman-Token': "dc28c10b-5858-4515-93b6-d86d789ff2a5"
}
sql =   """
        INSERT INTO vessel_tracking_log (device_id, receive_date, receive_time, longitude, latitude, msg_id, msg_detail, 
                                        created_at, updated_at) VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s)
        """

response = requests.request("POST", url, data=payload, headers=headers)
matches_data = re.finditer(regex_data, response.content, re.DOTALL)
mydb = mysql.connector.connect(
    host="192.168.254.33",
    user="root",
    passwd="FmcElcom!@#99",
    database="fmc_v3_11012019"
)
mycursor = mydb.cursor()

for matchNum, match_data in enumerate(matches_data, start=1):
    insertData = ()
    msgDetail = match_data.group()
    deviceID = None
    receiveDate = None
    receiveTime = None
    longitude = None
    latitude = None
    msgId = None
    createdAt = None
    updatedAt = None

    matches_value = re.finditer(regex_value, match_data.group())
    
    for matchNum, match_value in enumerate(matches_value, start=1):
        if len(match_value.groups()) < 2:
            continue
       
        if match_value.groups()[0] == 'DeviceID':
            deviceID = match_value.groups()[1]
            insertData = insertData + (deviceID,)

        if match_value.groups()[0] == 'ReceiveTime':
            tmpDatetime = dateutil.parser.parse(match_value.groups()[1])
            receiveDate = tmpDatetime.strftime('%s')
            receiveTime = tmpDatetime.strftime('%Y-%m-%d %H:%M:%S')
            insertData = insertData + (receiveDate,)
            insertData = insertData + (receiveTime,)

        if match_value.groups()[0] == 'Longitude':
            longitude = match_value.groups()[1]
            insertData = insertData + (longitude,)
        
        if match_value.groups()[0] == 'Latitude':
            latitude = match_value.groups()[1]
            insertData = insertData + (latitude,)
        
        if match_value.groups()[0] == 'MessageID':
            msgId = match_value.groups()[1]
            insertData = insertData + (msgId,)

    insertData = insertData + (msgDetail,)
    createdAt = datetime.now().strftime('%Y-%m-%d %H:%M:%S') 
    insertData = insertData + (createdAt,)
    updatedAt = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
    insertData = insertData + (updatedAt,)

    mycursor.execute(sql, insertData)
    mydb.commit()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
