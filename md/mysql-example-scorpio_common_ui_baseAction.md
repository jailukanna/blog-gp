---
title: mysql example scorpio common ui baseAction (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'scorpio common ui baseAction'


## python scorpio common ui baseAction

Python mysql example: scorpio common ui baseAction

```python
# coding; utf-8
from common.ui import *


class BaseAction(object):
    def __init__(self, selenium_driver):
        self.driver = selenium_driver  # type: DRIVER["type"]
        self.log = BaseLog(BaseAction.__name__).log

    def open(self, url):
        self.log.info("open url: %s", url)
        self.driver.get(url)

    def sleep(self, sleep_time=0.5):
        self.log.info("sleep: %.1fs", sleep_time)
        time.sleep(sleep_time)

    def find_element(self, *loc):
        self.log.info("find element by: " + str(loc))
        return self.driver.find_element(*loc)

    def script(self, src):
        self.log.info("execute script: %s", src)
        return self.driver.execute_script(src)

    def click(self, *loc, sleep=True):
        element = self.find_element(*loc)
        self.log.info("click element: %s", loc)
        element.click()
        if sleep:
            self.sleep()

    def send_keys(self, *loc, value, click_first=False, clear_first=False):
        if click_first:
            self.find_element(*loc).click()
            self.sleep()
        if clear_first:
            self.find_element(*loc).clear()

        element = self.find_element(*loc)
        self.log.info("send value: %s to element: %s", value, loc)
        element.send_keys(value)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
