---
title: mysql example t price (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 't price'

Functions in program: 
* `def get_stockName(stockCode):`
* `def get_stockID(stockCode):`

Modules used in program: 
* `import traceback`
* `import re`
* `import datetime`
* `import pylab as pl`
* `import matplotlib.pyplot as plt`
* `import sqlite3`
* `import mysql.connector`
* `import numpy as np`
* `import pandas as pd`

## python t price

Python mysql example: t price

```python
from futuquant import *
import pandas as pd
import numpy as np
import mysql.connector
import sqlite3
import matplotlib.pyplot as plt
import pylab as pl
import datetime
import re
from matplotlib.backends.backend_pdf import PdfPages
import traceback

quote_ctx = OpenQuoteContext(host='127.0.0.1', port=11111)

def get_stockID(stockCode):
    lines = quote_ctx.get_stock_basicinfo(Market.HK, SecurityType.STOCK)
    df = lines[1]
    stockId = (df.loc[df['code'] == stockCode])['stock_id'].to_string(index = False) 
    return stockId

def get_stockName(stockCode):
    lines = quote_ctx.get_stock_basicinfo(Market.HK, SecurityType.STOCK)
    df = lines[1]
    stockId = (df.loc[df['code'] == stockCode])['name'].to_string(index = False) 
    return stockId

cMysql = mysql.connector.connect(
         user='root',
         password='root',
         host='127.0.0.1',
         database='scrapy')
cursorMysql = cMysql.cursor()

sql = "SELECT * FROM aastock;"

cursorMysql.execute(sql)

results = cursorMysql.fetchall()



cMysql.close()

dayC = timedelta(days = 90)

connSqlite = sqlite3.connect("D:\\ft_hist_data\\hist_hk\hknor_kl_day_0.db")

connSqlite1 = sqlite3.connect("D:\\ft_hist_data\\hist_hk\hknor_kl_day_1.db")

cursorSqlite = connSqlite.cursor()

cursorSqlite1 = connSqlite1.cursor()

flag = 0

with PdfPages('multipage_pdf.pdf') as pdf:
    for line in results:
        try:
            stockCode = ('HK.'+line[2][0:5])
            flag = flag + 1
            print(str(flag) + ' ' + str(line[0]) + ' ' + stockCode )
            stockID = get_stockID('HK.'+line[2][0:5])

            stockName = get_stockName('HK.'+line[2][0:5])
            tPrice = re.findall(r"\d+\.?\d*",line[3])
            pDate = line[0]
            before_date = (line[0] + dayC)
            after_date = (line[0] - dayC)
            sql = """select date(time_desc) FROM KLData where 
                 stock_id = '%s' 
                 and  time_desc >= '%s' 
                 and time_desc <= '%s'""" % (stockID, after_date, before_date)
            cursorSqlite.execute(sql)
            x = cursorSqlite.fetchall()
            if len(x) == 0:
                cursorSqlite1.execute(sql)
                x = cursorSqlite1.fetchall()
                sql = """select highest_price FROM KLData where 
                 stock_id = '%s' 
                 and  time_desc >= '%s' 
                 and time_desc <= '%s'""" % (stockID, after_date, before_date)
                cursorSqlite1.execute(sql)
                y = cursorSqlite1.fetchall()
            else:
                sql = """select highest_price FROM KLData where 
                 stock_id = '%s' 
                 and  time_desc >= '%s' 
                 and time_desc <= '%s'""" % (stockID, after_date, before_date)
                cursorSqlite.execute(sql)
                y = cursorSqlite.fetchall()
            tDate = []
            cPrice = []
            for a in x:
                tDate.append(a[0])
            for b in y:
                cPrice.append(b[0])
            plt.cla()
            plt.rcParams['font.sans-serif']=['SimHei']
            plt.rcParams['axes.unicode_minus'] =False
            plt.title(stockCode + ' ' + stockName)
            plt.tick_params(bottom= False)
            plt.plot(tDate, cPrice)
            #plt.hlines(float(tPrice[0]), 0, max(y), colors = "c", linestyles = "dashed")
            plt.text(tDate[0], cPrice[0], line[4], size=30, rotation=30.,ha="left",
                va="bottom",bbox=dict(boxstyle="round",ec=(1., 0.5, 0.5),fc=(1., 0.8, 0.8),alpha=0.6))
            #plt.text(tDate[0], max(cPrice), (line[5]+'\n'+line[6]),size=10,ha="left",
            #    va="bottom",bbox=dict(boxstyle="round",ec=(1., 0.5, 0.5),fc=(1., 0.8, 0.8),alpha=0.6))
            plt.text(tDate[0], float(tPrice[0]), line[5],size=10,ha="left",
                va="bottom",bbox=dict(boxstyle="round",ec=(1., 0.5, 0.5),fc=(1., 0.8, 0.8),alpha=0.4))
            plt.xlabel('Date')
            plt.xticks([tDate[0],tDate[len(tDate)-1]])
            plt.ylabel('Price')
            plt.axvline(x=line[0].strftime('%Y-%m-%d'), ymin=0, ymax=1, hold=None, color = 'r', 
                linestyle = 'dashed', label= (line[1] + ' ' + line[0].strftime('%Y-%m-%d')))
            plt.axhline(y=float(tPrice[0]), xmin=0, xmax=1, hold=None, color = 'c', 
                linestyle = 'dashed', label=('Target Price'+' ' + tPrice[0]))
            plt.legend()
            #plt.show()
            #plt.savefig(str(flag) + '.png')
            pdf.savefig()
            plt.close()
        except Exception as e:
            pass
            print("""traceback.format_exc():\n%s""" % traceback.format_exc())


quote_ctx.close()




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
