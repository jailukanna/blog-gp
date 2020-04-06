---
title: mysql example connect (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'connect'

Functions in program: 
* `def end():`

Modules used in program: 
* `import mysql.connector as con`

## python connect

Python mysql example: connect

```python
import mysql.connector as con

cnx = con.connect(	user='mvp', 
					password='epidemiologia',
    	        	host='johnny.heliohost.org',
        	    	database='mvp_viral')
        	    	
cursor = cnx.cursor()

def end():
	cursor.close()
	cnx.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
