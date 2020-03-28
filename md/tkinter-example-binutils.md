---
title: tkinter example binutils (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'binutils'

Functions in program: 
* `def binDecodeString(binStr):`
* `def attemptDecode(numStr):`
* `def binEncodeString(rawString):`

Modules used in program: 
* `import re`

## python binutils

Python tkinter example: binutils

```python
import re

def binEncodeString(rawString):
  return ' '.join(
    map(lambda ch: str(bin(ord(ch))[2:]),
      rawString))

def attemptDecode(numStr):
  try:
    return str(chr(int(numStr, base=2)))
  except ValueError:
    pass
  except UnicodeEncodeError:
    pass

def binDecodeString(binStr):
  return ''.join(
    filter(None,
      list(map(attemptDecode,
        re.findall(r'[0-1]+', binStr)))))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
