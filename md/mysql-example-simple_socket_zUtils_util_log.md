---
title: mysql example simple socket zUtils util log (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'simple socket zUtils util log'


Modules used in program: 
* `import logging`

## python simple socket zUtils util log

Python mysql example: simple socket zUtils util log

```python
# coding=utf-8
import logging

# 定义并配置log变量
log = logging.getLogger('vehicle')
log.setLevel(logging.DEBUG)
hdr = logging.StreamHandler()
formatter = logging.Formatter('[%(asctime)s] %(name)s:%(levelname)s: %(message)s')
hdr.setFormatter(formatter)
log.addHandler(hdr)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
