---
title: mysql example redis%25E7%2589%2588 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'redis%25E7%2589%2588'


Modules used in program: 
* `import re`
* `import redis`

## python redis%25E7%2589%2588

Python mysql example: redis%25E7%2589%2588

```python
import redis
import re

red = redis.Redis(host='localhost', port=6379, db=0, decode_responses=True)

a = ''
while a != 'q':
    s = ''
    a = input().lstrip()
    if a == '':
        continue
    if a[0].isnumeric():
        for i in re.findall(r'.{4}', ''.join(a.split())):
            try:
                if red.get(i) is not None:
                    s += red.get(i)
            except:
                s = ''
        print(s)
    else:
        for i in a:
            if red.get(i) is not None:
                s += red.get(i) + ' '
        print(s)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
