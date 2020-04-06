---
title: mysql example web (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'web'


Modules used in program: 
* `import newrelic.agent`
* `import _mysql`

## python web

Python mysql example: web

```python
import _mysql
from os import system
from uuid import uuid4
from time import time as unix
from web import application as webapp
from web import header, ctx, data, input, notfound
import newrelic.agent

class enroll:
	def GET(self):
		action item here

class mdm:
	def PUT(self):
		do something here

	def POST(self):
		more action here

newrelic.agent.initialize('newrelic.ini')
sm = smobject()
urls = ('/enroll','enroll','/checkin','mdm','/server','mdm')
application = webapp(urls, globals()).wsgifunc()
application = newrelic.agent.WSGIApplicationWrapper(application)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
