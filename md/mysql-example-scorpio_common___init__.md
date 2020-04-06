---
title: mysql example scorpio common   init   (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'scorpio common   init  '


Modules used in program: 
* `import sys`

## python scorpio common   init  

Python mysql example: scorpio common   init  

```python
import sys

__PROJECT_NAME = "scorpio"

PROJECT_PATH = sys.path[0][0:sys.path[0].index(__PROJECT_NAME)] + __PROJECT_NAME

if __name__ == "__main__":
    print(PROJECT_PATH)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
