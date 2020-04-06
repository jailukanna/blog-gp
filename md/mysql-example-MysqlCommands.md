---
title: mysql example MysqlCommands (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'MysqlCommands'

Functions in program: 
* `def InsertMintData(data):`

Modules used in program: 
* `import mysql.connector`

## python MysqlCommands

Python mysql example: MysqlCommands

```python
import mysql.connector

def InsertMintData(data):
        
        id = data['id']
        lastUpdateInString = data['lastUpdatedInString']
        value = data['value']
        accountType = data['accountType']
        currentBalance = data['currentBalance']

        add_mintddatarow = ("INSERT INTO mintinfo (accountId,lastUpdatedInString,accountValue,accountType,currentBalance) VALUES(" + str(id) + ",'" + str(lastUpdateInString) + "'," + str(value) +",'" + str(accountType) +"'," + str(currentBalance) + ");")
        

        cnx = mysql.connector.connect(user='root', database='databasename', password='passwordhere', host='127.0.0.1')
        cursor = cnx.cursor()

        add_mintdatarow = ("INSERT INTO mintinfo (accountId,lastUpdatedInString,accountValue,accountType,currentBalance) VALUES(" + str(id),str(lastUpdateInString),str(value),str(accountType),str(currentBalance) + ");")

        # mint datarow
        mint_datarow = { data['accountId'], data['lastUpdatedInString'], data['value'], data['accountType'], data['currentBalance'] }
            

        # insert new datarow
        cursor.execute(add_mintdatarow)

        # make sure data is commited to the database
        cnx.commit()

        # close the connection
        cursor.close()
        cnx.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
