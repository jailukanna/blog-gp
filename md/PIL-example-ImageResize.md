---
title: PIL example ImageResize (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'ImageResize'


Modules used in program: 
* `import os`
* `import PIL`

## python ImageResize

Python PIL example: ImageResize

```python
import PIL
import os
from PIL import Image

basewidth = 1000

for f in os.listdir('./lrg'):
    img = Image.open('./lrg/' + f)
    wpercent = (basewidth / float(img.size[0]))
    hsize = int((float(img.size[1]) * float(wpercent)))
    img = img.resize((basewidth, hsize), PIL.Image.ANTIALIAS)
    img.save('./sml/' + f)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
