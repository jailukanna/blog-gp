---
title: pil example cuegen (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'cuegen'


## python cuegen

Python pil example: cuegen

```python
#!/usr/local/bin/python


TYPE = "wav"

BEGIN = 1
END = 7
STEP = 1


for i in range(BEGIN, END + 1, STEP):
    print(('FILE "{0:0>2}.{1}" WAVE\n  TRACK {0:0>2} AUDIO\n    ')
           'INDEX 01 00:00:00').format(i, TYPE)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
