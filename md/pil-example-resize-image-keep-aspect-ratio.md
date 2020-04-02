---
title: pil example resize-image-keep-aspect-ratio (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'resize-image-keep-aspect-ratio'


Modules used in program: 
* `import PIL`

## python resize-image-keep-aspect-ratio

Python pil example: resize-image-keep-aspect-ratio

```python
#Resizes an image and keeps aspect ratio. Set mywidth to the desired with in pixels.

import PIL
from PIL import Image

mywidth = 300

img = Image.open('someimage.jpg')
wpercent = (mywidth/float(img.size[0]))
hsize = int((float(img.size[1])*float(wpercent)))
img = img.resize((mywidth,hsize), PIL.Image.ANTIALIAS)
img.save('resized.jpg')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
