---
title: mysql example mysql data insert notification (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql data insert notification'


Modules used in program: 
* `import requests`
* `import mysql.connector`
* `import sys`

## python mysql data insert notification

Python mysql example: mysql data insert notification

```python
#encoding:utf-8
import sys
reload(sys)
sys.setdefaultencoding('utf8')

sitepackage = "D:\home\site\wwwroot\env\Lib\site-packages"
sys.path.append(sitepackage)

import mysql.connector
from mysql.connector import Error
import requests

htmlData=''

try:
    conn = mysql.connector.connect(host='xxx',
                                    database='xxx',
                                    user='xxxx',
                                    password='xxx')
    if conn.is_connected():
        print('Connected to MySQL database')

    cur = conn.cursor()
    cur.execute("select username,mobile,province,city,Stamp from xxxxx order by Stamp desc LIMIT 20")
    rows = cur.fetchall()
    print('Total Row(s):', cur.rowcount)

    for row in rows:
        htmlData+=str(row[0])+','+str(row[1])+','+str(row[2])+','+str(row[3])+','+str(row[4])+'<br/>'

    print(htmlData)
    #print("name=%s, mobile=%s,province=%s,city=%s,time=%s" % (row[0],row[1],row[2],row[3],row[4]))
except Error as e:
    print(e)

url = "http://api.sendcloud.net/apiv2/mail/send"

API_USER = 'xxxxx'
API_KEY = 'xxxxx'

params = {
"apiUser": API_USER, # 使用api_user和api_key进行验证
"apiKey" : API_KEY,
"to" : "xxxx@xxx.com;", # 收件人地址, 用正确邮件地址替代, 多个地址用';'分隔
"from" : "xxxx.mo@xxxx.com", # 发信人, 用正确邮件地址替代
"fromName" : "xxxxxxxxx",
"subject" : "xxxxxxxx",
"html": htmlData
}

r = requests.post(url, data=params)
print(r.text)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
