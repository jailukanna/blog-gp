---
title: mysql example flaskapi-config (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'flaskapi-config'


Modules used in program: 
* `import os`

## python flaskapi-config

Python mysql example: flaskapi-config

```python
from flaskext.mysql import MySQL
import os


from flask import Flask

app = Flask(__name__)

mysql = MySQL()

# MySQL configurations
app.config['MYSQL_DATABASE_USER'] = os.environ['db_username']
app.config['MYSQL_DATABASE_PASSWORD'] = os.environ['db_password']
app.config['MYSQL_DATABASE_DB'] = os.environ['db_name']
app.config['MYSQL_DATABASE_HOST'] = '192.168.1.213'
mysql.init_app(app)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
