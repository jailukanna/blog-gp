---
title: pil example sheet (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'sheet'


Modules used in program: 
* `import sys,PIL`

## python sheet

Python pil example: sheet

```python
"""
	Contact Sheet (Passport Size) 
Pass the image filename(s) as argument to the script and a pdf 
        contact sheet will be made 
by - arush15june
"""


import sys,PIL
from PIL import Image,ImageOps

width, height = int(8.27 * 300), int(11.7 * 300) # A4 at 300dpi
passWidth, passHeight = 1.37795, 1.77165 #inches

imFiles = sys.argv[1:]
for imFile in imFiles:
	image = Image.open(imFile)
	image = image.resize((int(300*passWidth),int(300*passHeight)),PIL.Image.ANTIALIAS) # Passport Size Photo 3.5cm x 4.5cm
	image = ImageOps.expand(image,border=int(300*0.12),fill='white')

	sheet = Image.new('RGB', (width, height), 'white')

	imHorizontal, imVertical = width/image.width, height/image.height

	x = 0
	y = 0

	print("Contact Sheet : ",imHorizontal*imVertical," Pictures")

	for i in range(0,imVertical):
		for j in range(0,imHorizontal):
			sheet.paste(image, box=(x, y))
			x += image.width
		y += image.height
		x = 0

	sheet.save('sheet-{}.pdf'.format(imFile))
	 

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
