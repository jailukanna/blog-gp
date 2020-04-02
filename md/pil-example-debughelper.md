---
title: pil example debughelper (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'debughelper'


Modules used in program: 
* `import logging`

## python debughelper

Python pil example: debughelper

```python
import logging

ch = logging.StreamHandler()

logger=logging.getLogger()
# add ch to logger
if logger.hasHandlers()==False:
	ch = logging.StreamHandler()
	logger.addHandler(ch)
	formatter = logging.Formatter('%(asctime)s -%(name)s: %(message)s')
	ch.setFormatter(formatter)
	
logger.setLevel(10)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
