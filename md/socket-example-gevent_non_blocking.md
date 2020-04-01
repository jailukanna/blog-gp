---
title: socket example gevent non blocking (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'gevent non blocking'

Functions in program: 
* `def fetch_request(url):`

Modules used in program: 
* `import requests`
* `import gevent`

## python gevent non blocking

Python socket example: gevent non blocking

```python
import gevent
from gevent import monkey
monkey.patch_all()

import requests

def fetch_request(url):
    print("fetching %s" % url)
    requests.get(url)
    print("Finish fetching %s" % url)


jobs = []
for url in ("http://google.com", "http://facebook.com"):
    jobs.append(gevent.spawn(fetch_request, url))

gevent.joinall(jobs)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
