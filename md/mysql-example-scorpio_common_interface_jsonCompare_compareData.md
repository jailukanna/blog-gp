---
title: mysql example scorpio common interface jsonCompare compareData (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'scorpio common interface jsonCompare compareData'


## python scorpio common interface jsonCompare compareData

Python mysql example: scorpio common interface jsonCompare compareData

```python
from common.util.jsonFormat import *


class CompareData(object):
    def __init__(self, code: int, data: dict, is_expect):
        self.code = code
        self.data = data
        self.is_expect = is_expect

    def __str__(self):
        if self.is_expect:
            return "Expect:\n\t%s data: %s" % (self.code, json_format(self.data))
        return "Actual:\n\t%s data: %s" % (self.code, json_format(self.data))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
