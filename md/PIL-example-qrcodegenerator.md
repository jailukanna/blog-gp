---
title: PIL example qrcodegenerator (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'qrcodegenerator'


Modules used in program: 
* `import qrcode`
* `import PIL`

## python qrcodegenerator

Python PIL example: qrcodegenerator

```python
import PIL
import qrcode
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFile

class QrcodeGenerator():

	def __init__ (self, data, text_overlay=True):
		self.data = data
		self.text_overlay = text_overlay

	def generateQrCode(self):
		for elems in self.data:
			self.saveQr(elems)

		return True

	def saveQr(self, data):
		img  = qrcode.make(data)
		if self.text_overlay:
			self.addTextOverImage(
				img, data
				)
		else:
			qrcode.make(
				data
				).save(data+".png")

		return True

	def addTextOverImage(self, img, txt):
		draw = ImageDraw.Draw(img)
		draw.text((5,5),txt)
		name = self.cleanUrl(txt)
		img.save(name+'.png')

		return True

	def cleanUrl(self, data):
		staging = []

		data = data.strip().split('http://')

		if len(data) > 1:
			staging.append(data[1])
		else:
			staging.append(data[0])

		#removing www, com, co, uk, etc
		for elems in staging:
			elems = elems.strip().split('.')
			if elems[0] == 'www':
				return elems[1]
			else:
				return elems[0]


Train_txt = ["Matrix", "Trinity", "Neo"]
Train_url = ["http://www.google.com", "http://facebook.co.uk","www.ebay.co.uk"]

x = QrcodeGenerator(Train_url, text_overlay=True)
x.generateQrCode()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
