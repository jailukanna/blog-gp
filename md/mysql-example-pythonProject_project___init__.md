---
title: mysql example pythonProject project   init   (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'pythonProject project   init  '


## python pythonProject project   init  

Python mysql example: pythonProject project   init  

```python
from flask import Flask
from flaskext.mysql import MySQL

mysql = MySQL()
app = Flask(__name__)

# MySQL configurations
app.config['MYSQL_DATABASE_USER'] = 'root'
app.config['MYSQL_DATABASE_PASSWORD'] = 'root'
app.config['MYSQL_DATABASE_DB'] = 'project'
app.config['MYSQL_DATABASE_HOST'] = 'localhost'
app.config['MYSQL_DATABASE_PORT'] = 8889
mysql.init_app(app)

conn = mysql.connect()
cursor = conn.cursor()

from . import login
from . import register


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
