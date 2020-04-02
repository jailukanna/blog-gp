---
title: pil example slicer (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'slicer'


Modules used in program: 
* `import PIL`

## python slicer

Python pil example: slicer

```python
#!/usr/bin/env python

#
# This Python script will crop a big image into a bunch of tiles of WIDTHxHEIGHT
# such that the whole original image will be covered. If the tiles don't go
# evenly into the original dimensions there will be overlaps.

# You need Pillow (the maintained fork of the Python Image Library) for this
# to run. "pip install Pillow" should do it.

import PIL
from PIL import Image

# PIL has a "decompression bomb" upper limit for image sizes: the next line
# turns it off because the LMC image is too big.

PIL.Image.MAX_IMAGE_PIXELS = None

INFILE = 'photo95f.jpg'
CONVERT = 'convert'

# size of the original image

IMG_WIDTH = 14400
IMG_HEIGHT = 14200

# size of the tiles you want

WIDTH = 2560
HEIGHT = 1920

XTILES = IMG_WIDTH // WIDTH + 1
XSPACE = (IMG_WIDTH - WIDTH ) // (XTILES - 1)
YTILES = IMG_HEIGHT // HEIGHT + 1
YSPACE = (IMG_HEIGHT - HEIGHT ) // (YTILES - 1)

xs = [ x * XSPACE for x in range(0, XTILES) ]
ys = [ y * YSPACE for y in range(0, YTILES) ]

orig = Image.open(INFILE)

for x in xs:
	for y in ys:
		outfile = "LMC_{0}x{1}.jpg".format(x, y)
		print("Cropping -> {}".format(outfile))
		tile = orig.crop((x, y, x + WIDTH, y + HEIGHT))
		tile.save(outfile)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
