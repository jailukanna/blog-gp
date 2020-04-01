---
title: socket example simple socket zUtils charset (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'simple socket zUtils charset'

Functions in program: 
* `def get_charset(str_orig):`

Modules used in program: 
* `import chardet`

## python simple socket zUtils charset

Python socket example: simple socket zUtils charset

```python
import chardet


def get_charset(str_orig):
    result = chardet.detect(str_orig)
    print(result)

get_charset("'{' \xb2\xbb\xca\xc7\xc4\xda\xb2\xbf\xbb\xf2\xcd\xe2\xb2\xbf\xc3\xfc\xc1\xee\xa3\xac\xd2\xb2\xb2\xbb\xca\xc7\xbf\xc9\xd4\xcb\xd0\xd0\xb5\xc4\xb3\xcc\xd0\xf2\n\xbb\xf2\xc5\xfa\xb4\xa6\xc0\xed\xce\xc4\xbc\xfe\xa1\xa3")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
