---
title: pil example ziddu (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'ziddu'


Modules used in program: 
* `import re`
* `import sys`

## python ziddu

Python pil example: ziddu

```python
#!/usr/bin/python
# Captcha breaker for ziddu
# Need tesseract-ocr installed
# Need PIL and pytesser module installed

# usage:
# $ python ziddu.py ziddu.jpeg 
# Tesseract Open Source OCR Engine v3.02.02 with Leptonica
# captcha: pswx5

from PIL import Image
from PIL import ImageEnhance
from pytesser import *
import sys
import re

gambar = sys.argv[1]
image = Image.open(gambar)
image = image.convert('L')
image.save('kapca2.jpg')

test = Image.open('kapca2.jpg')
img = ImageEnhance.Contrast(test)
img.enhance(1.5).save("kapca3.jpg")

img = Image.open("kapca3.jpg")
img = img.convert('RGBA')

pixdata = img.load()
for y in xrange(img.size[1]):
        for x in xrange(img.size[0]):
                if pixdata[x, y] >= (50, 50, 50, 255):
                        pixdata[x, y] = (255, 255, 255, 255)
img.save("kapca.gif","GIF")

img = Image.open("kapca.gif")
img.resize((300,120), Image.NEAREST)
img.save("kapca.tif")

image = Image.open('kapca.tif')
hasil = image_to_string(image)
final = re.sub("[^a-zA-Z0-9]+","",hasil)
print("captcha: {0}".format(final))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
