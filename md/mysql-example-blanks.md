---
title: mysql example blanks (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'blanks'

Functions in program: 
* `def metric_cleanup():`
* `def metric_init(params):`
* `def blank_logger(name):  `

Modules used in program: 
* `import re`
* `import time`
* `import _mysql`
* `import sys`

## python blanks

Python mysql example: blanks

```python
import sys
import _mysql
import time
import re

def blank_logger(name):  
	db=_mysql.connect(user="user",passwd="password",db="hitstats")
	db.query(""" SELECT sum(if(filename='nginx_timeout',totalhits,0)) blanks
	FROM `hitstats`.`serverhits` """)

    	r = db.use_result()
    	results = r.fetch_row()
    	sort_results = [results]
  	restr = str(sort_results)
	res = re.sub("[^^0-9]", "", restr)
  	return res 

def metric_init(params):
    global descriptors

    d1 = {'name': 'blanks',
        'call_back': blank_logger,
        'time_max': 900,
        'value_type': 'uint',
        'units': 'blanks',
        'slope': 'both',
        'format': '1',
        'description': 'Amount of blanks served',
        'groups': 'health'}

    descriptors = [d1]

    return descriptors

def metric_cleanup():
    '''Clean up the metric module.'''
    pass

#This code is for debugging and unit testing
if __name__ == '__main__':
    metric_init({})
    for d in descriptors:
        v = d['call_back'](d['name'])
        print('value for %s is %s' % (d['name'],  v))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
