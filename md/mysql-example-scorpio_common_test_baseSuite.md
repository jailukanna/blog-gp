---
title: mysql example scorpio common test baseSuite (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'scorpio common test baseSuite'


## python scorpio common test baseSuite

Python mysql example: scorpio common test baseSuite

```python
from common.test import *


class BaseSuite(object):
    def __init__(self, run_classes):
        self.run_classes = run_classes  # type: dict
        self.log = BaseLog(BaseSuite.__name__).log

    def run(self):
        print(1)
        self.log.info("\n\n Test Start \n ------------------------------------------------------------> ")
        print(2)

        suite = unittest.TestSuite()
        for case, is_run in self.run_classes.items():
            test = unittest.TestLoader().loadTestsFromTestCase(case)
            if is_run == 1:
                suite.addTest(test)
                self.log.info("Run Case: %s", test)
            elif is_run == 0:
                self.log.info("Not Run Case: %s", test)
                pass
            else:
                raise Exception("Wrong Arguments! 0:not run, 1:run")

        unittest.TextTestRunner().run(suite)
        self.log.info("\n\n <------------------------------------------------------------ \nTest End\n\n\n ")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
