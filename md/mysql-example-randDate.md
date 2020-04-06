---
title: mysql example randDate (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'randDate'

Functions in program: 
* `def randomDateP(start):`
* `def randomDate():`
* `def strTimeProp(start, end, format, prop):`

Modules used in program: 
* `import time`
* `import random`

## python randDate

Python mysql example: randDate

```python
import random
import time
from datetime   import timedelta, date

def strTimeProp(start, end, format, prop):
    """Get a time at a proportion of a range of two formatted times.

    start and end should be strings specifying times formated in the
    given format (strftime-style), giving an interval [start, end].
    prop specifies how a proportion of the interval to be taken after
    start.  The returned time will be in the specified format.
    """

    stime = time.mktime(time.strptime(start, format))
    etime = time.mktime(time.strptime(end, format))

    ptime = stime + prop * (etime - stime)

    return time.strftime(format, time.localtime(ptime))


def randomDate():
    return strTimeProp(str(date.today() + timedelta(days=-15)), str(date.today()), '%Y-%m-%d', random.random())
    
def randomDateP(start):
	return strTimeProp(start, str(date.today()), '%Y-%m-%d', random.random())


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
