---
title: pil example pil4pyg2 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'pil4pyg2'

Functions in program: 
* `def _pygame__image__load(*args):`
* `def open_(name):`
* `def pil2pygame(im):`

## python pil4pyg2

Python pil example: pil4pyg2

```python
from PIL import Image as pil_image
from pygame import image as pygame_image

def pil2pygame(im):
	return pygame_image.frombuffer(im.convert("RGBA").tostring(),im.size,"RGBA")
def open_(name):
	try:
		im=pil_image.open(name)
	except IOError,e:	#Compatibility with Pygame
		from pygame import base
		raise base.error(str(e))
	return pil2pygame(im)
def _pygame__image__load(*args):
	if len(args)==1:
		return open_(*args)
	else:
		return _old_pygame__image__load(*args)
_old_pygame__image__load,pygame_image.load=pygame_image.load,_pygame__image__load

pil_image.init()
exts=sorted(filter(lambda i:pil_image.EXTENSION[i] in pil_image.OPEN.keys(),pil_image.EXTENSION.keys()))




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
