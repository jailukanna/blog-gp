---
title: mysql example scorpio common interface httpUtils baseRequest (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'scorpio common interface httpUtils baseRequest'


## python scorpio common interface httpUtils baseRequest

Python mysql example: scorpio common interface httpUtils baseRequest

```python
from common.interface.httpUtils.httpMethod import HttpMethod


class BaseRequest(object):
    def __init__(self, url: str, method: HttpMethod):
        self.url = url
        self.method = method


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
