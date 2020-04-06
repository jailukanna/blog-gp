---
title: mysql example scorpio common test baseInterfaceTest (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'scorpio common test baseInterfaceTest'


## python scorpio common test baseInterfaceTest

Python mysql example: scorpio common test baseInterfaceTest

```python
from common.interface.httpUtils.httpUtil import *
from common.interface.jsonCompare.compare import CompareData, Comparator


class InterfaceTestBase(object):
    def __init__(self):
        self.log = BaseLog(InterfaceTestBase.__name__).log
        self.res = None  # type: ResponseItems

    def do_compare(self, request_items: RequestItems, expect_code: int, expect_json: dict, comparator=None):
        self.log.info("\n" + "=" * 100 + "\n" + request_items.__str__())
        self.res = do_request(request_items)
        self.log.info(self.res)

        expect = CompareData(expect_code, expect_json, True)
        actual = CompareData(self.res.status, self.res.json, False)
        self.log.info(expect)
        self.log.info(actual)

        if comparator is None:
            result = Comparator().compare(expect, actual)
        else:
            result = comparator.compare(expect, actual)
        self.log.info(result)

        if result.is_same is False:
            raise Exception("Run Fail!")
        else:
            self.log.info("Run Success!")

        import time
        time.sleep(1)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
