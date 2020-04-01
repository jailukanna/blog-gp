---
title: socket example test requests (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'test requests'


Modules used in program: 
* `import unittest  # todo: swap these over to py.test`

## python test requests

Python socket example: test requests

```python
import unittest  # todo: swap these over to py.test
from time import time

from .requests import requests, DEFAULT_REQUEST_TIMEOUT, DEFAULT_REQUEST_RETRIES


class test_Requests(unittest.TestCase):
    def test_001_set_request_timeout(self):
        from .requests import set_request_timeout

        self.assertEqual(requests.Session().request.keywords['timeout'], DEFAULT_REQUEST_TIMEOUT)

        set_request_timeout(1)
        self.assertEqual(requests.Session().request.keywords['timeout'], 1)

    def test_002_set_request_retries(self):
        from .requests import set_request_retries

        for adapter in requests.Session().adapters.values():
            assert adapter.max_retries.total == DEFAULT_REQUEST_RETRIES

        set_request_retries(0)
        for adapter in requests.Session().adapters.values():
            assert adapter.max_retries.total == 0

    def test_003_timeout(self):
        from .requests import requests, set_request_timeout, set_request_retries
        set_request_retries(0)
        set_request_timeout(1)
        start = time()
        with self.assertRaises(requests.ConnectionError):
            requests.get('https://httpbin.org/delay/5')
        self.assertLess((time() - start), 2)

        start = time()
        requests.get('https://httpbin.org/delay/2', timeout=4)
        elapsed = time() - start
        self.assertLess(elapsed, 4)
        self.assertGreater(elapsed, 2)

    def test_004_retries(self):
        from .requests import requests, set_request_retries, set_request_timeout
        set_request_timeout(0.5)
        set_request_retries(2)
        start = time()
        with self.assertRaises(requests.ConnectionError):
            requests.get('https://httpbin.org/delay/5')
        self.assertLess((time() - start), 3)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
