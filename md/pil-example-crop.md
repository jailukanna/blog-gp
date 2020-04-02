---
title: pil example crop (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'crop'


Modules used in program: 
* `import os`
* `import PIL`

## python crop

Python pil example: crop

```python
import PIL
from PIL import Image
import os

for x in os.listdir("./"):
    try:
        img = Image.open(x)
        w, h = img.size
        new_img = img.crop((100, 100, w-200, h-30))
        new_img.save(x)
    except:
        continue


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
