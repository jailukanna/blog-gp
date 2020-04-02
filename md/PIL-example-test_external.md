---
title: PIL example test external (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'test external'


Modules used in program: 
* `import urllib2`

## python test external

Python PIL example: test external

```python
import urllib2
from io import BytesIO
from PIL import Image

url = 'http://cdn1.www.st-hatena.com/users/da/daiiz/profile.gif?1451834248'

filePointer = urllib2.urlopen(url)
data = filePointer.read()
filePointer.close()

image = Image.open(BytesIO(data))
image.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
