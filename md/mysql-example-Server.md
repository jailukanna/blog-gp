---
title: mysql example Server (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Server'

Functions in program: 
* `def random(n):`
* `def execute_code(value):`
* `def update_file(value):`
* `def save_file():`

Modules used in program: 
* `import imp`
* `import json`
* `import os.path`
* `import os, uuid`
* `import mysql.connector`

## python Server

Python mysql example: Server

```python
import mysql.connector
import os, uuid
from bottle import request, run, post, route
from random import randint
import os.path
import json
import imp


shared_folder_path = '/mnt1/user_codes/'

mydb = mysql.connector.connect(
    host="localhost",
    user="root",
    passwd="root",
    database="codedatabase"
)


@post('/savecode')
def save_file():
    mycursor = mydb.cursor()
    code = request.body.read()
    try:
        compile(code, 'mulstring', 'exec')
        # Need to check if it has a method called handle with parameter request
        code_compiled = 1
    except:
        code_compiled = 0

    if code_compiled == 1:
        filename = shared_folder_path + str(uuid.uuid4()) + ".py"
        code_file = open(filename, "w+")
        code_file.write(code)
        sql = "insert into tbl_file_urls (fld_file_urls_code,fld_file_urls_url,fld_file_urls_isactive) values (%s,%s,%s)"
        val = (random(6), filename, True)
        mycursor.execute(sql, val)
        mydb.commit()
        mycursor.close()
        response = { "code_id" : val[0], "error_message" : ""}
    else:
        response = {"code_id": 0, "error_message": "Invalid Code"}

    return json.dumps(response)


@post('/updatecode/<value>')
def update_file(value):
    mycursor = mydb.cursor()
    code = request.body.read()
    try:
        compile(code, 'mulstring', 'exec')
        # Need to check if it has a method called handle with parameter request
        code_compiled = 1
    except:
        code_compiled = 0

    if code_compiled == 1:
        sql_select_query = "select * from tbl_file_urls where fld_file_urls_code="+value
        mycursor.execute(sql_select_query)
        row = mycursor.fetchone()
        if row is not None:
            code_file = open(row[2], 'w')
            code_file.write(code)
            response = {"code_id": row[1], "error_message": ""}
        else:
            response = {"code_id":0, "error_message": "Code doesn't exist"}
        mycursor.close()
    else:
        response = {"code_id": 0, "error_message": "Invalid Code"}

    return json.dumps(response)


@route('/executecode/<value>', method=['POST','GET'])
def execute_code(value):
    mycursor = mydb.cursor()
    sql_select_query = "select * from tbl_file_urls where fld_file_urls_code=" + value
    mycursor.execute(sql_select_query)
    row = mycursor.fetchone()
    if row is not None:
        code_file_url = row[2]
        mycursor.close()
        if os.path.isfile(code_file_url):
            f = open(code_file_url, "r")
            code = f.read()
            custom_module = imp.new_module('myfunctions')
            exec code in custom_module.__dict__
            response = custom_module.handle(request)
        else:
            response = {"code_id": 0, "error_message": "Code doesn't exist"}
    else:
        response = {"code_id": 0, "error_message": "Code doesn't exist"}
    return json.dumps(response)


def random(n):
    range_start = 10 ** (n - 1)
    range_end = (10 ** n) - 1
    return randint(range_start, range_end)


if __name__ == '__main__':
    run(host='localhost', port=4545, debug=True)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
