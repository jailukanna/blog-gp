---
title: mysql example gapi keyword (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'gapi keyword'

Functions in program: 
* `def run(ii, data, array, url, seed_table):`
* `def mysql_con(db):`
* `def prepare_credentials():`
* `def initialize_service():`

Modules used in program: 
* `import itertools`
* `import sys`
* `import time`
* `import mysql.connector`
* `import json`
* `import argparse`
* `import httplib2`

## python gapi keyword

Python mysql example: gapi keyword

```python
import httplib2
import argparse
import json
import mysql.connector
import time
import sys
import itertools

from joblib import Parallel, delayed
from apiclient.discovery import build
from apiclient import errors
from oauth2client.client import flow_from_clientsecrets
from oauth2client.file import Storage
from oauth2client import tools
from mysql.connector import errorcode
from itertools import chain

# 1st step in OAuth 2.0
def initialize_service():
    http = httplib2.Http()
    credentials = prepare_credentials()
    http = credentials.authorize(http)
    return build('webmasters', 'v3', http=http)

# 2nd step in OAuth 2.0
def prepare_credentials():
    parser = argparse.ArgumentParser(parents=[tools.argparser])
    flags = parser.parse_args()
    storage = Storage(TOKEN_FILE_NAME)
    credentials = storage.get()
    if credentials is None or credentials.invalid:
        credentials = tools.run_flow(FLOW, storage, flags)
    return credentials

# Basic MySQL connection ... now with exception handling
def mysql_con(db):
    try:
        con = mysql.connector.connect(user='root', password='', host='localhost', database='%s' % db)
        return con
    except mysql.connector.Error as e:
        print("Error code:", e.errno        # error number)
        print("SQLSTATE value:", e.sqlstate # SQLSTATE value)
        print("Error message:", e.msg       # error message)

def run(ii, data, array, url, seed_table):
    attempts = 0
    kw_query = None
    while attempts < 5:
        try:
            time.sleep(.2)
            request = {
              'startDate': start_date,
              'endDate': end_date,
              'searchType': 'web',
              'dimensions': ['query'],
              'dimensionFilterGroups': [{
                    'filters': [{
                              'dimension': 'query',
                              'operator': 'contains',
                              'expression': '%s' % ii[1]
                    }]
              }],
              'rowLimit': 5000
            }

            service = initialize_service()
            kw_query = service.searchanalytics().query(siteUrl=url, body=request).execute()
            break

        except errors.HttpError as e:
            print("\nAttempt # %s of 15\n" % attempts)
            time.sleep(1)
            attempts += 1
            print(e)

    print("\n-------------------------------\n", "Request filter: %s\n" %ii[1], "%s of %s\n" % (data.index(ii), len(data)),"-------------------------------\n")

    if kw_query['rows'] is not None:
        for iii in kw_query['rows']:
            try:
                query = iii['keys'][0]
                # imp = iii['impressions']
                # clicks = iii['clicks']
                # ctr = iii['ctr']
                # pos = iii['position']
                # # Must make query a unique key in SQL db to avoid duplicates
                # cursor.execute('''INSERT IGNORE INTO %s (query, impressions, clicks, ctr, position, start_date, end_date) VALUES (%%s, %%s, %%s, %%s, %%s, %%s, %%s)''' % keyword_table, (query, imp, clicks, ctr, pos, start_date, end_date))
                # cursor.execute('''INSERT IGNORE INTO %s (query, start_date, end_date) VALUES (%%s, %%s, %%s)''' % seed_table, (query, start_date, end_date))
                # con.commit()
                item = (query, start_date, end_date)
                array.append(item)
                print("Query: %s" % query)

            except mysql.connector.Error as e:
                print("Error code:", e.errno        # error number)
                print("SQLSTATE value:", e.sqlstate # SQLSTATE value)
                print("Error message:", e.msg       # error message)
                return
        print(array)
        return array
    else:
        print("No Data Loaded")
        return

# Start main
if __name__ == '__main__':

    # Edit this per use
    start_date = '2015-06-23'
    end_date = '2015-09-20'
    url = 'http://8tracks.com/'
    mysql_db = 'gapi'
    seed_table = 'seeds'
    keyword_table = 'stats'

    # Grab passwords and user info from local machine
    CLIENT_SECRETS = '/Users/mj/Documents/stuff/oauth.json'
    # Create a Flow object for OAuth 2.0
    FLOW = flow_from_clientsecrets(
        CLIENT_SECRETS,
        scope='https://www.googleapis.com/auth/webmasters.readonly',
        message='%s is missing' % CLIENT_SECRETS
        )
    # Generates a local Token file for Storage/OAuth 2.0
    TOKEN_FILE_NAME = 'credentials.dat'
    service = initialize_service()

    request = {
      'startDate': start_date,
      'endDate': end_date,
      'searchType': 'web',
      'dimensions': ['query'],
      'rowLimit': 5000
    }

    # Create a response, MySQL connection, and MySQL cursor
    response = service.searchanalytics().query(siteUrl=url, body=request).execute()
    con = mysql_con(mysql_db)


    if con and response:
        cursor = con.cursor()
        for i in response['rows']:
            try:
                query = i['keys'][0]
                cursor.execute('''INSERT IGNORE INTO %s (query, start_date, end_date) VALUES (%%s, %%s, %%s)''' % seed_table, (query, start_date, end_date))
                # print(query)
            except mysql.connector.Error as e:
                print("Error code:", e.errno        # error number)
                print("SQLSTATE value:", e.sqlstate # SQLSTATE value)
                print("Error message:", e.msg       # error message)

        # commit data to MySQL
        con.commit()
        print("Adding data to MySQL\n")

        while True:
            array = []
            ids = []
            cursor.execute('''SELECT id,query FROM %s WHERE processed = 0 LIMIT 10''' % seed_table)
            data = cursor.fetchall()
            for x in data:
                x_id = [x[0]]
                ids.append(x_id)

            try:
                cursor.executemany('''UPDATE %s SET processed = 1 WHERE id = %%s''' % seed_table, (ids))
                con.commit()
            except mysql.connector.Error as e:
                print("Error code:", e.errno        # error number)
                print("SQLSTATE value:", e.sqlstate # SQLSTATE value)
                print("Error message:", e.msg       # error message)
            # con.commit()

            results = Parallel(n_jobs=5)(delayed(run)(ii, data, array, url, seed_table) for ii in data)
            7
            result_list = list(itertools.chain.from_iterable(results))

            try:
                cursor.executemany('''INSERT IGNORE INTO %s (query, start_date, end_date) VALUES (%%s, %%s, %%s)''' % seed_table, (result_list))
                con.commit()
            except mysql.connector.Error as e:
                print("Error code:", e.errno        # error number)
                print("SQLSTATE value:", e.sqlstate # SQLSTATE value)
                print("Error message:", e.msg       # error message)

            cursor.execute('''SELECT query FROM %s WHERE processed = 0''' % seed_table)
            data = cursor.fetchall()

            if len(data) == 0:
                break
            else:
                print("###################################")
                print("%s remaining"% len(data))
                print("###################################")
                pass


cursor.close()
print("Ending mysql.connector cursor session")
con.close()
print("Ending MySQL connection")
sys.exit()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
