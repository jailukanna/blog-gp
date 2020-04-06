---
title: mysql example scorpio common log baseLog (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'scorpio common log baseLog'

Functions in program: 
* `def __set_level(config):`

Modules used in program: 
* `import logging`

## python scorpio common log baseLog

Python mysql example: scorpio common log baseLog

```python
# coding; utf-8
import logging

from config.logConfig import *


def __set_level(config):
    if config == 'DEBUG':
        return logging.DEBUG
    elif config == 'INFO':
        return logging.INFO
    elif config == 'WARNING':
        return logging.WARNING
    elif config == 'ERROR':
        return logging.ERROR


# file
_filehandler = logging.FileHandler(filename=LOG["file"]["path"], encoding="utf-8")
_filehandler.setLevel(__set_level(LOG["file"]["level"]))
_fmter = logging.Formatter(fmt=LOG["fommat"], datefmt=LOG["datefmt"])
_filehandler.setFormatter(_fmter)

# console
_console = logging.StreamHandler()
_console.setLevel(__set_level(LOG["console"]["level"]))
_formatter = logging.Formatter(LOG["fommat"])
_console.setFormatter(_formatter)


class BaseLog(object):
    def __init__(self, name: str):
        logging.basicConfig(level=logging.DEBUG, handlers=[_console, _filehandler])
        self.log = logging.getLogger(name)


if __name__ == '__main__':
    log = BaseLog("Test").log
    log.debug("this is debug msg")
    log.info("this is info msg")
    log.warning("this is warning msg")
    log.error("this is error msg")

    log.info("this is a format: %s %s", "hello", "world")

    log.info("this is info msg 爱爱爱")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
