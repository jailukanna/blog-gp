---
title: mysql example leadtime test (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'leadtime test'


Modules used in program: 
* `import datetime`
* `import tempfile`
* `import unittest`
* `import index`
* `import sys, os`

## python leadtime test

Python mysql example: leadtime test

```python
import sys, os
sys.path.append(os.path.dirname(os.path.abspath(__file__)) + '/../')
import index
import unittest
import tempfile
import datetime
from datetime import timedelta

class IndexTestCase(unittest.TestCase):

    def setUp(self):
        #self.db_fd, flaskr.DATABASE = tempfile.mkstemp()
        self.app = index.app.test_client()
        #flaskr.init_db()

    #def tearDown(self):
        #os.close(self.db_fd)
        #os.unlink(flaskr.DATABASE)

    def test_response(self):
        response_goodreq = self.app.get('/')
        self.assertEqual(response_goodreq.status_code, 200)
        response_badreq = self.app.get('/hoge')
        self.assertEqual(response_badreq.status_code, 404)

    def test_calc_leadtime_ave(self):
        lines = []
        lines = self.create_testcase_calc_leadtime_ave('2018/3/5 00:00:00', '2018/3/10 00:00:00', lines)
        result = index.calc_leadtime_ave(lines)
        self.assertEqual(result, 5.0)
        lines = self.create_testcase_calc_leadtime_ave('2018/2/28 00:00:00', '2018/3/10 00:00:00', lines)
        result = index.calc_leadtime_ave(lines)
        self.assertEqual(result, 7.5)

    # testcase生成
    # 第一引数はselfでないといけない
    # 呼び出し時にはselfは必要ない
    def create_testcase_calc_leadtime_ave(self, start, end, lines):
        start_datetime = datetime.datetime.strptime(start, "%Y/%m/%d %H:%M:%S")
        end_datetime   = datetime.datetime.strptime(end, "%Y/%m/%d %H:%M:%S")
        lines.append({"start_time": start_datetime, "end_time": end_datetime})
        return lines

if __name__ == '__main__':
    unittest.main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
