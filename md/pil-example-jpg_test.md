---
title: pil example jpg test (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'jpg test'


Modules used in program: 
* `import sys`
* `import os`
* `import PIL.Image`

## python jpg test

Python pil example: jpg test

```python
import PIL.Image
import os
import sys

imgpath = os.path.join(os.path.dirname(__file__), 'sample.jpg')

if not os.path.isfile(imgpath):
  print(imgpath, ' does not exist')
  sys.exit(1)

im = PIL.Image.open(imgpath)
im.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
