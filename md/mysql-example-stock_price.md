---
title: mysql example stock price (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'stock price'

Functions in program: 
* `def main():`
* `def get_data(symbol):`

Modules used in program: 
* `import os`
* `import pandas as pd`
* `import xml.etree.ElementTree as ET`
* `import mysql.connector`
* `import pandas_datareader.data as web`
* `import datetime`

## python stock price

Python mysql example: stock price

```python
'''
scrape all history stock Data
'''
import datetime
import pandas_datareader.data as web
from dateutil.relativedelta import relativedelta
import mysql.connector
import xml.etree.ElementTree as ET
import pandas as pd
import os


def get_data(symbol):
    start = datetime.datetime(2000, 01, 01);
    end = datetime.datetime.now();
    print('scraping [%s]' % symbol)
    # read the stock information from Google finance
    if symbol[0] == '^' or "VIX" in symbol :  # we have to call Stooq reader
        df = web.DataReader(symbol, 'stooq')
    else:
        df = web.DataReader(symbol, 'yahoo', start, end)

    # for backward compatible we have to add column called adj. price
    return df


'''Main Function'''
def main():
    os.chdir("/home/ec2-user/data/stock_price")
    # init mysql connection
    # mysql -h sfdata.cf6ulue4mzq9.us-west-2.rds.amazonaws.com -P 3306 -u spr1ngf0rward -p
    cnx = mysql.connector.connect(user='spr1ngf0rward', password='DTXpecgdTQzijTMg2',
                                  host='sfdata.cf6ulue4mzq9.us-west-2.rds.amazonaws.com',
                                  database='sfdata')
    # queries
    get_symbols = ("SELECT symbol_id, symbol FROM symbol WHERE exchange_id in (1,2,24)")
    Insert_Price = ("INSERT INTO stock_prices"
                     "(symbol, symbol_id, date, open, high, low, close, adj_close, volume, created_on)"
                     "VALUES (%(symbol)s, %(symbol_id)s, %(date)s, %(open)s, %(high)s, %(low)s, %(close)s, %(adj_close)s, %(volume)s, %(created_on)s)")
    symbols = []

    ## get symbol list
    cursor = cnx.cursor()
    cursor.execute(get_symbols)
    for (symbol_id, symbol) in cursor:
        symbols.append((symbol_id, symbol))
    cursor.close()

    # insert stock price into database
    output = 'stock_prices.csv' 
    # debug
    # symbols = [(1, 'AABA'), (2, 'YHOO'), (3, 'BABA')]
    for (symbol_id, symbol) in symbols:
        try:
            data=get_data(symbol)
            data["symbol"] = symbol
            data["symbol_id"] = symbol_id
            data["date"] =data.index
            data["created_on"]= datetime.datetime.now().date()
            order = ['symbol', 'symbol_id', 'date', 'Open', 'High', 'Low', 'Close', 'Adj Close', 'Volume', 'created_on']
            data = data.ix[:, order]
            data.to_csv(output, index = False, mode = 'a', header = False)
            print(("download %s success" % (symbol)))
        except Exception as ex:
            print(("download %s fails - %s" % (symbol, ex)))


if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
