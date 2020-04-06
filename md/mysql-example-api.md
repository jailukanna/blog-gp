---
title: mysql example api (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'api'

Functions in program: 
* `def getnews():`
* `def get_data(category,limit,c):`
* `def default(o):`
* `def get_db(db=db):`
* `def connectDb(dbname):`

Modules used in program: 
* `import datetime`
* `import json`
* `import  mysql.connector`

## python api

Python mysql example: api

```python
from flask import Flask,request,g
from settings import dbpassword, dbusername
import  mysql.connector
import json
import datetime


app = Flask(__name__)




categories = ["news","lifestyle","story","video","world","collumn","business","entertainment","lifestyle","sports","report","news"]


#create connection with db
def connectDb(dbname):
	print(dbusername,dbpassword)
	db = mysql.connector.connect(
		host="127.0.0.1",
		user= dbusername,
		password = dbpassword,
		database = dbname
		)
	return(db)


db = connectDb("articleDb")



def get_db(db=db):
    """Opens a new database connection if there is none yet for the
    current application context.
    """
    if not hasattr(g, 'conn'):
        print("dont have")
        g.conn = connectDb("articleDb")
    return g.conn


#handle time
def default(o):
    if isinstance(o, (datetime.date, datetime.datetime)):
        return o.isoformat()


#create function to gte data from db
#no validation will done here all validatiion will done by request handler
def get_data(category,limit,c):
    
    if (category =="all"):
        sql = "SELECT * FROM `content_db` ORDER BY `content_db`.`datetime` DESC LIMIT {} ".format(limit)

        c.execute(sql)
        results =c.fetchall()
        return results

    else:
        sql = "SELECT * FROM `content_db` WHERE category =  '{1}' ORDER BY `content_db`.`datetime` DESC  LIMIT {0} ".format(limit,category)
        c.execute(sql)
        results = c.fetchall()
        return results






@app.route("/getnews", methods=["GET"])
def getnews():
    
    cnx = get_db().cursor(dictionary=True,buffered=True)

    categories = ["news","lifestyle","story","video","world","collumn","business","entertainment","lifestyle","sports","report","news"]




    category = request.args.get("category")
    limit = request.args.get("limit")

    if category in categories and limit.isdigit():
        data = get_data(category,limit,cnx)
        jsonData = json.dumps(data,default=default)

        return jsonData

    elif category == "all" and limit.isdigit():
        data = get_data("all",50,cnx)
        cnx.close()
        jsonData = json.dumps(data,default=default)
        return jsonData
    
    else:
        return "invalid response"


    








if __name__ == '__main__':
    app.run('0.0.0.0', 5555,debug=True)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
