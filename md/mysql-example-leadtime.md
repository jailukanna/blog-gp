---
title: mysql example leadtime (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'leadtime'

Functions in program: 
* `def db_connect():`
* `def calc_leadtime_ave(lines):`
* `def leadtime_daily(name, d, d_op, start_datetime, end_datetime):`
* `def leadtime():`

Modules used in program: 
* `import sys`
* `import datetime`
* `import json`
* `import numpy as np`
* `import mysql.connector`

## python leadtime

Python mysql example: leadtime

```python
from flask import Flask, request, redirect, url_for
import mysql.connector
import numpy as np
import json
import datetime
from datetime import timedelta
import sys
# pip install python-dateutil
from dateutil.relativedelta import relativedelta

app = Flask(__name__)

@app.route('/')
def leadtime():
    name  = "service1"
    d     = "1d"
    d_op  = d[-1]
    d     = int(d[0]) * 7 if d[1] == "w" else int(d[0]) 
    start = '2017/11/20 00:00:00'
    end   = '2018/3/10 00:00:00'
    # notice: 後述する str"ft"time ではなく str"pt"time
    start_datetime = datetime.datetime.strptime(start, "%Y/%m/%d %H:%M:%S")
    end_datetime   = datetime.datetime.strptime(end, "%Y/%m/%d %H:%M:%S")

    return leadtime_daily(name, d, d_op, start_datetime, end_datetime)

def leadtime_daily(name, d, d_op, start_datetime, end_datetime):
    ret  = []
    conn = db_connect()
    cur  = conn.cursor(dictionary=True)
    sql   = "select * from sample where name = %s and start_time >= %s and start_time < %s and end_time < %s"
    try:
        while start_datetime < end_datetime:
            # get (start ~ start + [d]day) data
            # if end_time = null, we skip such line
            if(d_op == "d" or d_op == "w"):
                start_datetime_add_delta = start_datetime + timedelta(d)
            elif(d_op == "m"):
                start_datetime_add_delta = start_datetime + relativedelta(months = d)
            else:
                raise 
            # SQLの実行
            cur.execute(sql, (
                name,
                start_datetime.strftime("%Y/%m/%d %H:%M:%S"),
                start_datetime_add_delta.strftime("%Y/%m/%d %H:%M:%S"),
                end_datetime.strftime("%Y/%m/%d %H:%M:%S")
                ))
            # 平均の計算
            leadtime_ave = calc_leadtime_ave(cur.fetchall())
 
            # JSON形式に整形
            ret.append({"date": start_datetime.strftime("%Y/%m/%d"), "leadtime": round(leadtime_ave, 3)})
            start_datetime = start_datetime_add_delta
        return json.dumps({
            "status": "ok",
            "result": {
                "type": "leadtimeAve",
                "data": ret
                }
            })
    except Exception as e:
        return json.dumps({
            "status": "ng",
            "result": {
                "type": "error",
                "data": ""
                }
            })
    finally:
        cur.close()
        conn.close()

def calc_leadtime_ave(lines):
    leadtime_sum = timedelta(0) # 初期化
    count = 0
    for line in lines:
        # notice: start=2/25 12:00:00 end=2/26 00:00:00 の場合 end-start = 0.5(日)となる  
        leadtime_sum += line["end_time"] - line["start_time"]
        count += 1
    return (leadtime_sum / timedelta(count)) if count != 0 else 0

def db_connect():
    conn = mysql.connector.connect(
        host = 'localhost',
        port = 3306,
        user = 'root',
        password = 'hogehoge',
        database = "practice"
    )
    return conn

if __name__ == '__main__':
    #app.debug = True
    app.run(host='0.0.0.0') # host='0.0.0.0'は重要



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
