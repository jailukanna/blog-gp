---
title: pil example jpg2png2invert (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'jpg2png2invert'


Modules used in program: 
* `import PIL.ImageOps`

## python jpg2png2invert

Python pil example: jpg2png2invert

```python
from PIL import Image
import PIL.ImageOps
#Using PIL/pillow (install with 'pip3 install pillow' ) 
name = 'RPS-g2.jpg'
outname = 'RPS-g2.png' #png extension
im = Image.open(name)
inv = PIL.ImageOps.invert(im) #invert opperation
inv.save(outname)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
