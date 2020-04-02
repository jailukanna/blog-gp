---
title: PIL example negate all bmp (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'negate all bmp'


Modules used in program: 
* `import glob`
* `import PIL.ImageOps`

## python negate all bmp

Python PIL example: negate all bmp

```python
from PIL import Image
import PIL.ImageOps
import glob
for f in glob.glob('./*.bmp'):
    image = Image.open(f).convert("RGB")
    inverted_image = PIL.ImageOps.invert(image)
    inverted_image.save(f)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
