---
title: mysql example simple constrictor application (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'simple constrictor application'


Modules used in program: 
* `import os, sys`

## python simple constrictor application

Python mysql example: simple constrictor application

```python
# Suggested directory structure:
# /myapp/this_file.py
# /constrictor

# This is to load the path above it to add constrictor directory to the path.
import os, sys
sys.path.append(os.path.join(os.getcwd(), os.pardir))

# Recommended to make pathing easier. (Note: Requires os module)
from constrictor.utils import set_path
set_path(os.getcwd())

# Load constrictor's MySQL ORM system.
from constrictor.database import mysql
# Connect to the database.
database = mysql.mysql('localhost', 'root', '', 'blog')

# New version, not as elegant, but more functional and understandable.
class UserModel(mysql.model):
  table = 'users'
  structure = [
    mysql.fields.integer('id', primary = True, null = False, auto_increment = True),
    mysql.fields.string('username', length = 32),
    mysql.fields.string('password', length = 32),
    mysql.fields.string('fullname', length = 32),
    mysql.fields.string('email', length = 32),
    mysql.fields.string('website', length = 32),
    # Password is a special fields used to make working with passwords easier.
    # Algorithm can either be a string defining a standard hash or a class that  
    # that acts like a standard hashlib library
    # mysql.fields.password('password', algorithm = 'sha256')
    #mysql.fields.boolean('is_admin', null = False)
  ]
# Actually registering the model with the database system.
database.register.model(UserModel)

#u = UserModel()

# A test query.
#result = database.query('SELECT * FROM users')

# Import constrictor and other stuff (Expose, etc.)
from constrictor.core import *
# Initialize a new constrictor instance.
constrictor = Constrictor()

view = """
<html>
<head><title>Test</title></head>
<body>
Users:
<%?
for user in result:
  OUTPUT(str('<b>' + user.username + '</b><br/ >'))
%>
</body>
</html>
"""

class Controller(Constrictor.Controller):
  @Expose
  def method(self, req, args):
    print("second!")
    from constrictor.template import repl
    result = database.query('SELECT * FROM users')
    template = repl.repl()
    return (200, template.render(view, locals()))
  @Filter.Before
  def method2(self, req, args):
    #print(self)
    #print(req)
    #print(args)
    print("first!")
  @Filter.After
  def method3(self, req, args):
    print("third!")
c = Controller()

constrictor.routes = [
  Constrictor.Route(r'^/([A-z0-9-_]+)/?([A-z0-9-_]+)?/?', Controller.method),
  Constrictor.Route(r'^/$', Controller.method)
]

# Import and start the actual server
from constrictor.server.core import Server
server = Server('localhost', 8080, constrictor, False)

# Remembering to close the connection.
database.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
