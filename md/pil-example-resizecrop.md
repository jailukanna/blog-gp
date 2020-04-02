---
title: pil example resizecrop (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'resizecrop'

Functions in program: 
* `def resizecrop(src, out, width, height):`

Modules used in program: 
* `import PIL`

## python resizecrop

Python pil example: resizecrop

```python
import PIL
from PIL import Image

def resizecrop(src, out, width, height):
	img = Image.open(src)
	src_w = float(img.size[0])
	src_h = float(img.size[1])
	ratio = max(width/src_w, height/src_h)
	out_w = int(src_w * ratio)
	out_h = int(src_h * ratio)
	out_t = (out_h - height) / 2
	out_l = (out_w - width) / 2
	out_r = out_l + width
	out_b = out_t + height
	img = img.resize((out_w, out_h), PIL.Image.ANTIALIAS)
	img = img.crop((out_l, out_t, out_r, out_b))
	img.save(out)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
