---
title: pil example test (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'test'


Modules used in program: 
* `import sys`
* `import itertools`
* `import PIL`

## python test

Python pil example: test

```python

'''
python task1.py path/to/input threshold-int
'''


import PIL
from PIL import Image
import itertools
import sys
from PIL import ImageDraw
image = Image.open(sys.argv[1])
image = image.convert("L") # convert to signle channeled image

width, height = image.size



pixels = image.load() # allows pixel values to be edited
for x,y in itertools.product(range(width), range(height)): # all possible values of x and y
	if pixels[x,y] > int(sys.argv[2]):
		pixels[x,y] = 255
	else:
		pixels[x,y] = 0




image.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
