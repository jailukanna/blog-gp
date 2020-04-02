---
title: PIL example build folder from photo (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'build folder from photo'


Modules used in program: 
* `import shutil`
* `import os`
* `import sys`
* `import glob`

## python build folder from photo

Python PIL example: build folder from photo

```python
import glob
import sys
import os
import shutil
from PIL import Image
from PIL.ExifTags import TAGS

folder = glob.glob(sys.argv[1] + "/*")
for i in folder:
    try:
        exif = Image.open(i)._getexif()
        for id,val in exif.items():
            tag = TAGS.get(id,id)
            if tag != "DateTimeOriginal":
                continue
            dest = sys.argv[1] + '/' + val[:10].replace(':', '-')
            os.makedirs(dest, exist_ok=True)
            Image.open(i).close()
            shutil.move(i, dest + '/' + os.path.basename(i))
            break
    except:
        continue

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
