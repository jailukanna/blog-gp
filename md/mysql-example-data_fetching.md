---
title: mysql example data fetching (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'data fetching'

Functions in program: 
* `def main():`
* `def get_incre_option_data(csvpath):`
* `def get_incre_stock_prices():`
* `def get_all_option_data(csvpath):`
* `def get_all_stock_prices(csvpath):`
* `def get_all_symbols():`
* `def get_option_data(symbol):`
* `def get_stock_price(symbol, start, end):`

Modules used in program: 
* `import pandas_datareader.data as web`
* `import xml.etree.ElementTree as ET`
* `import mysql.connector`
* `import json, requests`
* `import os, time, datetime`

## python data fetching

Python mysql example: data fetching

```python
import os, time, datetime
import json, requests
import mysql.connector

import xml.etree.ElementTree as ET
from dateutil.relativedelta import relativedelta

import pandas_datareader.data as web
from pandas_datareader.data import Options

def get_stock_price(symbol, start, end):
    # read the stock information from Google finance
    if symbol[0] == '^' or "VIX" in symbol:  # we have to call Stooq reader
        df = web.DataReader(symbol, 'stooq')
    else:
        df = web.DataReader(symbol, 'yahoo', start, end)

    # for backward compatible we have to add column called adj. price
    return df

def get_option_data(symbol):
    stock_option = Options(symbol, "yahoo")
    data = stock_option.get_all_data()
    data.reset_index(inplace=True)
    adj_last = []
    price_data = data.ix[:, ['Bid', 'Ask']]
    last = data["Last"]
    data = data.drop(["Chg", "PctChg", "IV", "Root", "IsNonstandard", "Underlying", "Underlying_Price", "Quote_Time",
             "Last_Trade_Date", "JSON"], 1)
    for i in range(data.shape[0]):
        if last[i] in list(price_data.ix[i, :]):
             adj_last.append(last[i])
        else:
            adj_last.append(sum(price_data.ix[i, :]) / 2)
    data["Adj_last"] = adj_last
    return data

def get_all_symbols():
    # init mysql connection
    # mysql -h sfdata.cf6ulue4mzq9.us-west-2.rds.amazonaws.com -P 3306 -u spr1ngf0rward -p
    cnx = mysql.connector.connect(user='spr1ngf0rward', password='DTXpecgdTQzijTMg2',
                                  host='sfdata.cf6ulue4mzq9.us-west-2.rds.amazonaws.com',
                                  database='sfdata')
    # queries
    get_symbols = ("SELECT symbol_id, symbol FROM symbol WHERE exchange_id in (1,2,24)")
    symbols = []

    ## get symbol list
    cursor = cnx.cursor()
    cursor.execute(get_symbols)
    for (symbol_id, symbol) in cursor:
        symbols.append((symbol_id, symbol))
    cursor.close()
    return symbols

def get_all_stock_prices(csvpath):
    # hard-code the start date for now
    start = datetime.datetime(2017, 07, 30);
    end = datetime.datetime.now();
    os.chdir(csvpath)
    symbols = get_all_symbols()
    # insert stock price into database
    output = 'stock_prices.csv'
    # debug
    # symbols = [(1, 'AABA'), (2, 'YHOO'), (3, 'BABA')]
    for (symbol_id, symbol) in symbols:
        try:
            data = get_stock_price(symbol, start, end)
            data["symbol"] = symbol
            data["symbol_id"] = symbol_id
            data["date"] = data.index
            data["created_on"] = datetime.datetime.now().date()
            order = ['symbol', 'symbol_id', 'date', 'Open', 'High', 'Low', 'Close', 'Adj Close', 'Volume', 'created_on']
            data = data.ix[:, order]
            for s_key in list(data.columns):
                data[s_key] = data[s_key].fillna(method='ffill')
                data[s_key] = data[s_key].fillna(method='bfill')
                data[s_key] = data[s_key].fillna(1.0)
            data.to_csv(output, index=False, mode='a', header=False)
            print(("download %s success" % (symbol)))
        except Exception as ex:
            print(("download %s fails - %s" % (symbol, ex)))

def get_all_option_data(csvpath):
    os.chdir(csvpath)
    # init mysql connection
    # mysql -h sfdata.cf6ulue4mzq9.us-west-2.rds.amazonaws.com -P 3306 -u spr1ngf0rward -p
    cnx = mysql.connector.connect(user='spr1ngf0rward', password='DTXpecgdTQzijTMg2',
                                  host='sfdata.cf6ulue4mzq9.us-west-2.rds.amazonaws.com',
                                  database='sfdata')
    symbols = get_all_symbols()
    output = 'options.csv'
    for (symbol_id, symbol) in symbols:
        try:
            data = get_option_data(symbol)
            data["Symbol"] = symbol
            data["symbol_id"] = symbol_id
            data["created_on"] = datetime.datetime.now().date()
            # print(data.head(5))
            order = ['symbol_id', 'Strike', 'Expiry', 'Type', 'Symbol', 'Last', 'Bid', 'Ask', 'Vol', 'Open_Int',
                     'Adj_last', 'created_on']
            data = data.ix[:, order]
            data = data[(data.Vol >= 20) & (data.Last > 0.5) & (data.Open_Int >= 10)]
            # print(data.head(5))
            data.to_csv(output, index=False, mode='a', header=False)
            print(("download %s success" % (symbol)))
        except:
            print(("download %s fails" % (symbol)))

def get_incre_stock_prices():
    # init mysql connection
    # mysql -h sfdata.cf6ulue4mzq9.us-west-2.rds.amazonaws.com -P 3306 -u spr1ngf0rward -p
    cnx = mysql.connector.connect(user='spr1ngf0rward', password='DTXpecgdTQzijTMg2',
                                  host='sfdata.cf6ulue4mzq9.us-west-2.rds.amazonaws.com',
                                  database='sfdata')
    # queries
    get_symbols = ("SELECT symbol_id, symbol FROM symbol WHERE exchange_id in (1,2,24)")
    Insert_yesterday = ("INSERT INTO stock_prices"
                        "(symbol, symbol_id, date, open, high, low, close, adj_close, volume, created_on)"
                        "VALUES (%(symbol)s, %(symbol_id)s, %(date)s, %(open)s, %(high)s, %(low)s, %(close)s, %(adj_close)s, %(volume)s, %(created_on)s)")
    symbols = get_all_symbols()
    start = datetime.datetime.now() - relativedelta(days=1)
    end = start
    for (symbol_id, symbol) in symbols:
        try:
            cursor = cnx.cursor()
            data = get_stock_price(symbol, start, end)
            # Insert salary information
            Stock_price = {
                "symbol": symbol,
                "symbol_id": symbol_id,
                "date": datetime.datetime.now().date() - relativedelta(days=1),
                "open": float(data.ix[0, "Open"]),
                "high": float(data.ix[0, "High"]),
                "low": float(data.ix[0, "Low"]),
                "close": float(data.ix[0, "Close"]),
                "adj_close": float(data.ix[0, 'Adj Close']),
                "volume": int(data.ix[0, "Volume"]),
                "created_on": datetime.datetime.now().date()
            }
            cursor.execute(Insert_yesterday, Stock_price)
            # Make sure data is committed to the database
            cnx.commit()
            cursor.close()
            print(("insert %s done" % (symbol)))
        except:
            print(("insert %s fails" % (symbol)))


def get_incre_option_data(csvpath):
    def get_incre_option_data(csvpath):
        os.chdir(csvpath)
        # init mysql connection
        # mysql -h sfdata.cf6ulue4mzq9.us-west-2.rds.amazonaws.com -P 3306 -u spr1ngf0rward -p
        cnx = mysql.connector.connect(user='spr1ngf0rward', password='DTXpecgdTQzijTMg2',
                                      host='sfdata.cf6ulue4mzq9.us-west-2.rds.amazonaws.com',
                                      database='sfdata')
        symbols = get_all_symbols()
        output = 'data.sql'
        fs = open(output, 'w')
        for (symbol_id, symbol) in symbols:
            try:
                data = get_option_data(symbol)
                data["Symbol"] = symbol
                data["symbol_id"] = symbol_id
                data["created_on"] = datetime.datetime.now().date()
                order = ['symbol_id', 'Strike', 'Expiry', 'Type', 'Symbol', 'Last', 'Bid', 'Ask', 'Vol', 'Open_Int',
                         'Adj_last', 'created_on']
                data = data.ix[:, order]
                data = data[(data.Vol >= 20) & (data.Last > 0.5) & (data.Open_Int >= 10)]
                for i in data.index:
                    strike = str(data.ix[i, "Strike"]);
                    expiry = "\"" + str(data.ix[i, "Expiry"].date()) + "\"";
                    type = "\"" + str(data.ix[i, "Type"]) + "\"";
                    last = str(data.ix[i, "Last"]);
                    bid = str(data.ix[i, "Bid"]);
                    ask = str(data.ix[i, "Ask"]);
                    volume = str(data.ix[i, "Vol"]);
                    open_int = str(data.ix[i, "Open_Int"]);
                    adj_last = str(data.ix[i, "Adj_last"]);
                    created_on = "\"" + str(data.ix[i, "created_on"]) + "\"";
                    str1 = "INSERT INTO options (strike, expiry, type, symbol, symbol_id, last, bid, ask, volume, open_int, adj_last, created_on) VALUES "
                    str2 = "(%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s) " % (
                    strike, expiry, type, "\"" + symbol + "\"", symbol_id, last, bid, ask, volume, open_int, adj_last, created_on)
                    str3 = "ON DUPLICATE KEY UPDATE symbol_id=%s, last=%s, bid=%s, ask=%s, volume=%s, open_int=%s, adj_last=%s, created_on=%s;" % (
                    symbol_id, last, bid, ask, volume, open_int, adj_last, created_on)
                    string = str1 + str2 + str3
                    fs.write(string + "\n")
                print(("download %s success" % (symbol)))
            except:
                print(("download %s fails" % (symbol)))
        fs.close()


def main():
    """
    Main function of this module
    """
    from optparse import OptionParser
    usage = "usage: this tool is to fetch stock prices and option data."
    parser = OptionParser(usage=usage)
    # parser.add_option("-s", "--symbol",
    #                  dest="symbol", default="",
    #                  help="symbol of the stock, e.g GOOG")
    parser.add_option("-t", "--type",
            type='choice',
            dest='type',
            choices=['stock', 'option'],
            default="stock",
            help="data type: stock or option")
    parser.add_option("-m", "--mode",
            type='choice',
            dest='mode',
            choices=['batch', 'incremental'],
            default="batch",
            help="fetch mode: batch or incremental")
    parser.add_option("-o", "--output",
            dest="output", 
            default="/tmp",
            help="output csv file path for batch fetching")

    (options, args) = parser.parse_args()

    #symbol = options.symbol
    datatype = options.type
    mode = options.mode
    output = options.output

    if not datatype or not mode:
        print("type -h to see help")
        exit(-1)
    if datatype == "stock" and mode == "batch":
        get_all_stock_prices(output)
    if datatype == "stock" and mode == "incremental":
        get_incre_stock_prices()
    if datatype == "option" and mode == "batch":
        get_all_option_data(output)
    if datatype == "option" and mode == "incremental":
        get_incre_option_data(output)

    #get_all_option_data("/Users/zhenliu/stock_price")


if __name__ == "__main__":
    main()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
