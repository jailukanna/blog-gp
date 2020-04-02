---
title: PIL example orange2blue (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'orange2blue'


Modules used in program: 
* `import glob`
* `import PIL.ImageOps`

## python orange2blue

Python PIL example: orange2blue

```python
from PIL import Image
import PIL.ImageOps
import glob
orange = (255,102,0)
blue =(30,86,160)
for f in glob.glob('./*.bmp'):
    img = Image.open(f).convert("RGB")
    pixdata = img.load()
    for y in xrange(img.size[1]):
        for x in xrange(img.size[0]):
            pix = pixdata[x, y]
            delta = (pix[0] - orange[0], pix[1] - orange[1],
                     pix[2] - orange[2])
            maxdelta = sum(abs(d) for d in delta)
            if maxdelta < 300:
                pixdata[x, y] = (blue[0] + delta[0],
                                 blue[1] + delta[1],
                                 blue[2] + delta[2])
    img.save(f)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
