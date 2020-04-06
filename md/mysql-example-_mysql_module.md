---
title: mysql example  mysql module (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example ' mysql module'


Modules used in program: 
* `import sys`
* `import _mysql`

## python  mysql module

Python mysql example:  mysql module

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import _mysql
import sys

try:
	con = _mysql.connect('localhost', 'testuser', '123456', 'testdb')

	con.query('select version()')
	result = con.use_result()

	print('MySQL version: %s' % \)
		result.fetch_row()[0]

except _mysql.Error, e:

	print("Error %d: %s" % (e.args[0], e.args[1]))
	sys.exit(1)

finally:

	if con:
		con.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
