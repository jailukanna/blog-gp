---
title: PIL example filter2 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'filter2'


## python filter2

Python PIL example: filter2

```python
from PIL import Image
from PIL import ImageFilter
from PIL import ImageEnhance
from PIL import ImageOps
from PIL import ImageDraw

im = Image.open("park.jpg")

px = im.load()

for x in range(0,im.width):
	for y in range(0,im.height):	
		R = px[x,y][0]
		G = px[x,y][1]
		B = px[x,y][2]

		if R < 100:
			R = R + 100

		if G > 100:
			G = G - 50 

		if B > 100:
			B = B - 100

		px[x,y] = (R, G, B)



im = im.filter(ImageFilter.GaussianBlur(1))
im = im.filter(ImageFilter.EDGE_ENHANCE)

mask = Image.new( 'L', (im.width, im.height), 0)
draw = ImageDraw.Draw(mask)
draw.ellipse( (((im.width/2)-150),25,((im.width/2)+150),325), fill=255)

output = Image.new ('RGB', (im.width, im.height), (255,255,255))

output.paste(im, (0,0), mask)

output.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
