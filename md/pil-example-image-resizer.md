---
title: pil example image-resizer (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'image-resizer'

Functions in program: 
* `def image_resizer(path):`

Modules used in program: 
* `import PIL`
* `import os`

## python image-resizer

Python pil example: image-resizer

```python
import os
import PIL

from PIL import Image


def image_resizer(path):
    img = Image.open(path)
    
    basewidth = 700
    wpercent = (basewidth / float(img.size[0]))
    # hsize = int((float(img.size[1]) * float(wpercent)))
    hsize = 300
    
    img = img.resize((basewidth, hsize), PIL.Image.ANTIALIAS)
    img.save(path)

if __name__ == '__main__':
    BASE_DIR = os.path.dirname(os.path.dirname(__file__))
    image_path = os.path.join(BASE_DIR, 'your-image.jpeg')
    image_resizer(image_path)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
