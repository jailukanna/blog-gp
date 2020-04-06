---
title: mysql example scorpio common test baseUiTest (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'scorpio common test baseUiTest'


## python scorpio common test baseUiTest

Python mysql example: scorpio common test baseUiTest

```python
from common.test import *

from common.ui.uiDriverManager import *


class UITestBase(unittest.TestCase):
    def setUp(self):
        self.manager = UIDriverManager()
        self.driver = self.manager.get_driver()  # type: DRIVER["type"]
        self.log = BaseLog(UITestBase.__name__).log

    def tearDown(self):
        self.manager.close(self.driver)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
