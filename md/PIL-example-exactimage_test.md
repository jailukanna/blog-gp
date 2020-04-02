---
title: PIL example exactimage test (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'exactimage test'

Functions in program: 
* `def bulkResize(imageFolder, factor):`
* `def resize(folder, fileName, factor):`

Modules used in program: 
* `import Image`
* `import sys`
* `import os`

## python exactimage test

Python PIL example: exactimage test

```python
#!/usr/bin/python

import os
import sys
import Image

def resize(folder, fileName, factor):
    filePath = os.path.join(folder, fileName)

    im = Image.open(filePath)
    w, h  = im.size

    os.system( "econvert -i " + filePath + " --quality 90 --scale " + str(factor) + " -o " + filePath+"_copy_ei.jpg")

def bulkResize(imageFolder, factor):
    imgExts = ["png", "bmp", "jpg"]
    for path, dirs, files in os.walk(imageFolder):
        for fileName in files:
            ext = fileName[-3:].lower()
            if ext not in imgExts:
                continue

            resize(path, fileName, factor)

if __name__ == "__main__":
    imageFolder=sys.argv[1] # first arg is path to image folder
    resizeFactor=float(sys.argv[2])/100.0# 2nd is resize in %
    bulkResize(imageFolder, resizeFactor)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
