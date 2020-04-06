---
title: mysql example ImportCsvToMySQL (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'ImportCsvToMySQL'

Functions in program: 
* `def generate_sql(csvfile, spamreader, connection):`
* `def execute_sql(query, cnx):`
* `def trim_last_character(instvalues):`
* `def any(iterable):`

Modules used in program: 
* `import logging.config`
* `import logging`
* `import sys`
* `import time`
* `import MySQLdb`
* `import csv, sys, pprint`
* `import mysql.connector`
* `import os`

## python ImportCsvToMySQL

Python mysql example: ImportCsvToMySQL

```python
#!/usr/bin/python

from __future__ import print_function
from datetime import date, datetime, timedelta
import os
import mysql.connector
import csv, sys, pprint
import MySQLdb
import time
import sys
import logging
import logging.config




def any(iterable):
    for element in iterable:
        if element:
            return element
    return False


def trim_last_character(instvalues):
    if len(instvalues) > 0:
       if instvalues[-1:] == ",":
          #instvalues =  instvalues.encode('utf-32')
          instvalues = instvalues[:-1]
    return instvalues
	
def execute_sql(query, cnx):

    cursor = cnx.cursor()
    cursor.execute(query)
    cnx.commit()
    return cursor.rowcount


def generate_sql(csvfile, spamreader, connection):

    sql=""
    instvalues = ""

    x = 0	
    
    #print(instvalues)			   
    sqlHeader = "INSERT INTO " + data[2] + " ("
    cursor = connection.cursor()
    
    #reset position file
    csvfile.seek(0)	
    headers = spamreader.next()
    
    #headers for
    newHeaders = []  
    
    #Define header need to be removed
    removedHeader = []  
    addedHeader = []

    
    for header in headers:
        qShowColumns = "show columns FROM " + data[2] + " where field = '" +  header + "' or field = '" +  header[:-2] + "'"
        cursor.execute(qShowColumns)
        row = cursor.fetchone()

        if row != None:
            sqlHeader += " " + row[0] + ","
            addedHeader.append({
                'name' : row[0], 
                'index': headers.index(header)
            })
        else:
            removedHeader.append({
                'name' : header, 
                'index': headers.index(header)
            })

    success = 0
    failed = 0
    duplicate = 0
    failedId = []
    for row in spamreader:
        x += 1
#        if(x % 1000 == 0):
#            print(x)
        instvalues += "("
        #check whether primary key exists or not
        
        qCheckId = "select id from " + data[2] + " where id = '" + row[0] + "' "
        cursor.execute(qCheckId)
        currentRow = cursor.fetchone() 
        removed = ""
        
        try:
            if currentRow == None:
                instvalues = ""
                for header in addedHeader:
					if(row[header['index']] == ''):
						instvalues +=  "null,"
					else:
						instvalues +=  "'"+ MySQLdb.escape_string(row[header['index']]) 
						instvalues += "'" if len(addedHeader) == addedHeader.index(header) + 1 else "',"
                instvalues = trim_last_character(instvalues) + ");"
                sql = trim_last_character(sqlHeader) + ") VALUES (" + instvalues + "\n"
                #print((removed))
                #exit()
                if execute_sql(sql, cnx = connection) == 1 :
                    success += 1
                    #print((success))
                else:
                    failed += 1
                    print((row[0]))
                    #print((sql))
            else:
                duplicate += 1
                
                #print((str(row[0]) + " Exists");)
        except Exception, e:
            print((e))
            #continue
            failedId.append(row[0])
            failed += 1
            


    return {
        'failed' : failed, 
        'success' : success, 
        'duplicate' : duplicate,
        'failedId' : failedId
    }	
	
#READ CSV FILE
data = sys.argv 
cnx = mysql.connector.connect(user='root', database='dbName', port=3306, buffered = True, password='')
with open(data[1], 'rb') as csvfile:
    startTime = time.time()
    print("Reading CSV File ...")
    spamreader = csv.reader(csvfile, delimiter=',', quotechar='"')
    print("Inserting Csv File ...")	
    result = generate_sql(csvfile,spamreader, connection = cnx)
    elapsedTime = time.time() - startTime;
    print('Finish \n')
    result['executionTime'] = "{:.2f}".format(elapsedTime) + " s"
    result['recordpersecond'] = "{:.2f}".format((result['failed'] + result['success'] + result['duplicate']) / elapsedTime)
    pp = pprint.PrettyPrinter(indent=4)
    pp.pprint(result)
	


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
