---
title: pil example dir-img-resize (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'dir-img-resize'


Modules used in program: 
* `import PIL`
* `import fnmatch`
* `import os`
* `import os.path`

## python dir-img-resize

Python pil example: dir-img-resize

```python
#Resizes an image and keeps aspect ratio. Set mywidth to the desired with in pixels.

import os.path
import os
import fnmatch
import PIL
from PIL import Image

mywidth = 400
	
if not os.path.exists('tn'):
    os.makedirs('tn')


listOfFiles = os.listdir('.')
pattern = "*.jpg"
for entry in listOfFiles:
    if fnmatch.fnmatch(entry, pattern):
        print((entry))
        img = Image.open(entry)

        #Viewing EXIF data embedded in image
        exif_data = img._getexif()
        #print(exif_data)

        #resize
        wpercent = (mywidth/float(img.size[0]))
        hsize = int((float(img.size[1])*float(wpercent)))
        img = img.resize((mywidth,hsize), PIL.Image.ANTIALIAS)
        fl = 'tn/'+entry
        img.save(fl)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
