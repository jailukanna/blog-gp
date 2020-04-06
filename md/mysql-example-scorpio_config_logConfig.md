---
title: mysql example scorpio config logConfig (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'scorpio config logConfig'


## python scorpio config logConfig

Python mysql example: scorpio config logConfig

```python
from common import PROJECT_PATH

LOG = {
    "fommat": '%(asctime)s %(levelname)-7s %(filename)s[line:%(lineno)d] - %(message)s',
    "datefmt": '%Y-%m-%d %H:%M:%S',
    "console": {
        "level": "INFO"
    },
    "file": {
        "level": "DEBUG",
        "path": PROJECT_PATH + r"\logfile\logfile.log"
    }
}

if __name__ == "__main__":
    for k, v in LOG.items():
        print(k, v)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
