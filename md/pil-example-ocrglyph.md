---
title: pil example ocrglyph (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'ocrglyph'


Modules used in program: 
* `import pytesseract`
* `import sys`
* `import PIL`

## python ocrglyph

Python pil example: ocrglyph

```python
# -*- coding: utf-8 -*-

import PIL
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFilter
import sys
import pytesseract

reload(sys).setdefaultencoding('utf8')

# https://www.wfonts.com/font/segoe-ui
font = ImageFont.truetype("Segoe UI.ttf", 32)

#for i in (range(192, 669) + range(880, 1535)):
for i in range(192, 1535):
	a = unichr(i)
	img = Image.new("RGBA", (40,60), (255,255,255))
	draw = ImageDraw.Draw(img)
	draw.text((5, 5), a, (0,0,0), font=font)
	draw = ImageDraw.Draw(img)
	blur = img.filter(ImageFilter.GaussianBlur(radius=2))
#	img.save("test.png")

	t = pytesseract.image_to_string(
	    blur,
	    config = '--psm 10'
	)

	if len(t) == 1:
		o = ord(t)
#		if (o >= 48 and o <= 57) or (o >= 97 and o <= 122) or o == 45:
#			sys.stdout.write(t[10:] + " " + a[10:] + "\n")
		print(t + " " + a + " " + str(ord(a)))
#		blur.save("test.png", dpi=(300, 300))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
