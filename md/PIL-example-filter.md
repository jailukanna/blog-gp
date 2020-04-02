---
title: PIL example filter (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'filter'


## python filter

Python PIL example: filter

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

		if R > 50:
			R = R - 30

		if B < 200:
			B = B + 30

		px[x,y] = (R, G, B)

for x in range(0,im.width):
	for y in range(0,im.height):
		R = px[x,y][0]
		G = px[x,y][1]
		B = px[x,y][2]

		if R > 100:
			R = R - 99

		if B > 100:
			B = B - 99

		if G > 100:
			G = G - 99

		px[x,y] = (R, G, B)

for x in range(0,im.width):
	for y in range(0,im.height):
		R = px[x,y][0]

		if R > 50:
			R = R - 15

		px[x,y] = (R, G, B)

im = im.filter(ImageFilter.GaussianBlur(1))
im = im.filter(ImageFilter.EDGE_ENHANCE_MORE)

mask = Image.new( 'L', (im.width, im.height), 0)
draw = ImageDraw.Draw(mask)
draw.ellipse( (100,25,(mask.width-200), (mask.height-50)), fill=255)

output = Image.open("park.jpg")

output.paste(im, (0,0), mask)

output.show()

output.save("penny.jpg")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
