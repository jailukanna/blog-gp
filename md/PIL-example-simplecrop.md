---
title: PIL example simplecrop (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'simplecrop'

Functions in program: 
* `def cropall(img):`
* `def crop(data,bgcolor = [255,255,255]):`

Modules used in program: 
* `import console`
* `import photos`
* `import numpy as np`
* `import clipboard as clip`
* `import PIL as pil`

## python simplecrop

Python PIL example: simplecrop

```python
# coding: utf-8
# for pythonista

import PIL as pil
from PIL import ImageOps
import clipboard as clip
import numpy as np
import photos
import console

def crop(data,bgcolor = [255,255,255]):
	count = 0
	for row in data:
		count += 1
		for v in row:
			if cmp(bgcolor, list(v)):
				return count

def cropall(img):
	
	w, h = img.size
	temp = img.convert('RGB')
	data = np.array(temp)
	
	cropup = crop(data)
	cropdown = crop(np.flipud(data))
	cropright = crop(np.rot90(data))
	cropleft = crop(np.rot90(data,3))
	
	return sprite.crop((cropleft,cropup,w-cropright,h-cropdown))

console.clear()
sprite = clip.get_image()

sprite = cropall(sprite)
sprite.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
