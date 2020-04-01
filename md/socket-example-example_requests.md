---
title: socket example example requests (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'example requests'

Functions in program: 
* `def run():`

Modules used in program: 
* `import requests`

## python example requests

Python socket example: example requests

```python
import requests
from common import URL, check

def run():
    response = requests.get(URL)
    return response.status_code, response.headers, response.content

if __name__ == '__main__':
    check(*run())


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
