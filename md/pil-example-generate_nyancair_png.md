---
title: pil example generate nyancair png (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'generate nyancair png'

Functions in program: 
* `def decode(imageio):`
* `def encode(data, imageio):`

Modules used in program: 
* `import re`
* `import PIL`
* `import zlib`
* `import base64`
* `import imageio`
* `import requests`

## python generate nyancair png

Python pil example: generate nyancair png

```python
from PIL import Image
from cStringIO import StringIO
import requests
import imageio
import base64
import zlib
import PIL
import re

def encode(data, imageio):
	img = Image.open(imageio)
	pix = img.load()
	data = base64.b64encode(zlib.compress(data, 9))
	y = 1
	for byte in data:
		pix[y,y] = (pix[y,y][0], pix[y,y][1], ord(byte))
		y = y + 1
		pass
	pix[0,0] = (0, 1, 1)
	image_byte_array = StringIO()
	img.save(image_byte_array, format='PNG')
	return image_byte_array

def decode(imageio):
	img = Image.open(imageio)
	stego_pix = img.load()
	byte_list = []
	for y in range(1, img.size[1]):
		byte_list.append(chr(stego_pix[y,y][2]))
	pass
	return zlib.decompress(base64.b64decode(''.join(byte_list).strip('\0')))

eicar = 'X5O!P%@AP[4\PZX54(P^)7CC)7}'
eicar += '$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*'

nyan_cat_link = "http://aboutcolonblank.com/wp-content/uploads/2012/07/nyan-cat.png"
nyan_cat_io = StringIO(requests.get(nyan_cat_link).content)

evil_nyan_cat = encode(eicar, nyan_cat_io)

decoded_data = decode(evil_nyan_cat)

print(decoded_data)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
