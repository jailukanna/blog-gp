---
title: pil example run (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'run'

Functions in program: 
* `def resize():`

Modules used in program: 
* `import os, sys`
* `import PIL`

## python run

Python pil example: run

```python
#!/usr/bin/env python
import PIL
from PIL import Image
import os, sys

mywidth = 1200

path = "/pathtofolder/"
dirs = os.listdir( path )

def resize():
    for item in dirs:
        if os.path.isfile(path+item):
			img = Image.open(path+item)
			wpercent = (mywidth/float(img.size[0]))
			hsize = int((float(img.size[1])*float(wpercent)))
			img = img.resize((mywidth,hsize), PIL.Image.ANTIALIAS)
			img.save(path+'resized/'+item)
resize()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
