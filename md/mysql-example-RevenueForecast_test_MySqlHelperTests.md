---
title: mysql example RevenueForecast test MySqlHelperTests (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'RevenueForecast test MySqlHelperTests'


Modules used in program: 
* `import os`
* `import logging`
* `import unittest`

## python RevenueForecast test MySqlHelperTests

Python mysql example: RevenueForecast test MySqlHelperTests

```python
import unittest
import logging
import os
from db_helper.mysql_helper import MySqlHelper


class MySqlHelperTest(unittest.TestCase):
    def setUp(self):
        logging.basicConfig(level=logging.INFO)
        self.base_dir = os.path.join(os.path.dirname(os.path.realpath(__file__)), 'test_data')
        if not os.path.exists(self.base_dir):
            os.makedirs(self.base_dir)

    def tearDown(self):
        pass

    def testForecast(self):
        mysql_h = MySqlHelper()
        r = mysql_h.query("SELECT * FROM account_historical_revenue LIMIT 10;")
        logging.info(r)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
