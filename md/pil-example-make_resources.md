---
title: pil example make resources (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'make resources'

Functions in program: 
* `def main():`

Modules used in program: 
* `import sys`
* `import shutil`
* `import PIL.Image`
* `import os.path`
* `import glob`

## python make resources

Python pil example: make resources

```python
#!/usr/bin/env python                                                                                                                                                                                    
#                                                                                                                                                                                                        
# Simple script that uses PIL to automatically generate both 1x and 2x resolution                                                                                                                        
# PNGs from a source directory of 2x files. Handy for iPhone/iPad retina assets.                                                                                                                         
#                                                                                                                                                                                                        

import glob
import os.path
import PIL.Image
import shutil
import sys

def main():
    if len(sys.argv) != 3:
        print("usage: make_resources sourcedir destdir")
        sys.exit(1)

    srcdir = sys.argv[1]
    dstdir = sys.argv[2]
    files = glob.glob(os.path.join(srcdir, "*.png"))
    for filename in files:
        base = os.path.splitext(os.path.basename(filename))[0]
        dstpath2x = os.path.join(dstdir, base + "@2x.png")
        shutil.copyfile(filename, dstpath2x)

        img = PIL.Image.open(filename)
        dstpath1x = os.path.join(dstdir, base + ".png")
        newsize = [x / 2 for x in img.size]
        smallimg = img.resize(newsize)
        smallimg.save(dstpath1x)

        print("%s -> %s, %s" % (filename, dstpath1x, dstpath2x))

main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
