---
title: mysql example loadExample (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'loadExample'

Functions in program: 
* `def insertRows(pair1, vwap, buyPct, sellPct, rangePct): #values passed in from ticker API call`

Modules used in program: 
* `import mysql.connector`

## python loadExample

Python mysql example: loadExample

```python
# load csvs into sql (adding symbol column)

import mysql.connector

def insertRows(pair1, vwap, buyPct, sellPct, rangePct): #values passed in from ticker API call
    # This function will insert the data from the file into a table in our database
    
    #delimiter = r','
    # files need to be csv
    query = "INSERT INTO VWAPdata(timestamp, symbol, vwap, buyPct, sellPct, rangePct)"\
            "VALUES(CURRENT_TIMESTAMP,%s,%s,%s,%s,%s)"#, symbol = %s"
    params = (pair1, vwap, buyPct, sellPct, rangePct)
            
    
    config = {
        'user':'root',
        'password':'B*********3',
        'host':'127.0.0.1',
        'database':'P**********a'
    }
    
    conn = mysql.connector.connect(**config)
    c = conn.cursor()
    c.execute(query,params)
    conn.commit()
    

#finally:
    c.close()
    conn.close()
    
    return

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
