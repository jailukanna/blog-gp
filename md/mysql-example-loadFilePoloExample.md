---
title: mysql example loadFilePoloExample (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'loadFilePoloExample'

Functions in program: 
* `def insertRows(fileName,currency,c)`

Modules used in program: 
* `import os`
* `import mysql.connector`

## python loadFilePoloExample

Python mysql example: loadFilePoloExample

```python
# load csvs into sql (adding symbol column)

import mysql.connector

config = {
    'user':'root',
    'password':'B********3',
    'host':'127.0.0.1',
    'database':'P********a'
}

conn = mysql.connector.connect(**config)

c=conn.cursor()

def insertRows(fileName,currency,c)
    
    delimiter = r','
    
    c.execute("Load data local infile %s into table pairs fields terminated by %s ignore 1 lines"
                "(volume, quoteVolume, high, low, @timestamp, close, weightedAvg, open, symbol)"
                "SET timestamp = FROM_UNIXTIME(@timestamp), symbol = %s",(fileName,delimiter,currency))
                

localExtractFilePath='/Users/josephpalumbo/Desktop/HD Documents/PoloMarginPairsTrading'

import os

# loop through multiple files to build table
for currency in pairsList: # pairsList is a list of currency symbols
    for file in os.listdir(localExtractFilePath):
        if file.startswith(currency):
            insertRows(localExtractFilePath+"/"+file,currency,c)
            print("Loaded file "+file+" into database")
            conn.commit()
            
c.close()
conn.close()

# separate path for loading BTC_USDT at a later time
insertRows(localExtractFilePath+"/BTC_USDT1095_Days900_Period.csv","BTC_USDT",c)
conn.commit()
c.close()
conn.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
