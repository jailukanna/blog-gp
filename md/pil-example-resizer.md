---
title: pil example resizer (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'resizer'


Modules used in program: 
* `import PIL, sys, os`

## python resizer

Python pil example: resizer

```python
# sample usage:
# python resizer.py ./path ./resized 150

import PIL, sys, os
from PIL import Image

path = sys.argv[1]
path_resized = sys.argv[2]
base_width = int(sys.argv[3])

files = [os.path.join(dp, f) for dp, dn, filenames in os.walk(path) for f in filenames]
for file in files:
    try:
        img = Image.open(file)
        wpercent = (base_width/float(img.size[0]))
        hsize = int((float(img.size[1])*float(wpercent)))
        img = img.resize((base_width,hsize), PIL.Image.ANTIALIAS)
        try:
            img.save(path_resized + "/" + file) 
        except:
            t = path_resized +  "/" + file
            path_to_create = "/".join(t.split("/")[:-1])
            os.makedirs(path_to_create)
            img.save(path_resized + "/" + file) 
        print((file + " resized."))
    except Exception as e:
        print(e)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
