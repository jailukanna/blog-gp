---
title: pil example imgconv (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'imgconv'

Functions in program: 
* `def ui_to_pil(src):`
* `def pil_to_ui(src):`

## python imgconv

Python pil example: imgconv

```python
from io import BytesIO
from PIL import Image as pimg
from ui import Image as uimg

def pil_to_ui(src):
	with BytesIO() as bio:
		src.save(bio, "PNG")
		out = uimg.from_data(bio.getvalue())
	del bio
	return out
	
def ui_to_pil(src):
	with BytesIO(src.to_png()) as bio:
		og_size = (int(src.size.width), int(src.size.height))
		out = pimg.open(bio).resize(og_size, pimg.ANTIALIAS)
	del bio
	return out


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
