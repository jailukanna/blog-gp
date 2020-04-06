---
title: mysql example streaming app (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'streaming app'

Functions in program: 
* `def get_top_five():`
* `def index():`

Modules used in program: 
* `import json`
* `import mysql.connector`

## python streaming app

Python mysql example: streaming app

```python
from flask import Flask
from flask import render_template
import mysql.connector
from flask import jsonify
import json




app = Flask(__name__)

@app.route("/")
def index():
    return render_template("index.html")


@app.route("/top5")
def get_top_five():
    #cnx = mysql.connector.connect(user='dennis', password='Focus8Strength',host='127.0.0.1', database='tweetsdb')
    #cursor = cnx.cursor()
    #cursor.execute('select word, cnt from tweets order by cnt desc limit 5')
    #rows = cursor.fetchall()
    #cursor.close()
    #cnx.close()
    data = [1,2,3,4,5]
    return jsonify(data=rows)

if __name__ == "__main__":
    app.run(host='0.0.0.0',port=5000,debug=True)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
