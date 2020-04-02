---
title: pil example search (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'search'

Functions in program: 
* `def getPicture(sender):`
* `def savePicture(sender):      # create. test image file`
* `def updatePicture(sender):`
* `def btn2_tapped(sender):`
* `def btn1_tapped(sender):`
* `def pil_to_ui_image(img):`

Modules used in program: 
* `import io`
* `import photos`
* `import ui`

## python search

Python pil example: search

```python
import ui
import photos
import io
from PIL import Image

from debughelper import logger
from sqconnect import testReadOne,testWriteOne,testThreadWrite,TestPhoto


def pil_to_ui_image(img):
	bio = io.BytesIO()
	img.save(bio, 'PNG')
	data = bio.getvalue()
	bio.close()
	return ui.Image.from_data(data)
			
testPhoto=TestPhoto()
	

def btn1_tapped(sender):
	assets = photos.pick_asset(title='Pick some assets', multi=True)
	print(assets)
	last_assets = assets[-1]
	logger.debug('a')
	img = last_assets.get_ui_image()
	logger.debug('b')
	imageview=sender.superview['imageview1']
	imageview.image=img
	testPhoto.name='hage'
	testPhoto.comment='easy'
	testPhoto.trueImage=False
	testPhoto.mainImage=last_assets.get_image()
	logger.debug('c')
	#logger.debug('gygy')
	
@ui.in_background
def btn2_tapped(sender):
	img = photos.capture_image()
	#print(assets)
	#last_assets = assets[-1]
	print(img)
	imgt=img.copy()
	imgt.thumbnail((320,240))
	uiimage=pil_to_ui_image(imgt)
	
	testPhoto.mainImage=img
	imageview=sender.superview['imageview1']
	imageview.image=uiimage
	testPhoto.name='hage'
	testPhoto.comment='easy'
	testPhoto.trueImage=False

def updatePicture(sender):
	testThreadWrite(testPhoto)
	
def savePicture(sender):      # create. test image file
	testPhoto.mainImage=sender.superview['imageview1']
	im=Image.open(io.BytesIO(imageview.image.to_png()))
	
	im.save('temp.png','PNG')
	
def getPicture(sender):
	testReadOne(testPhoto,1)    #just make sure to read data
	sender.superview['imageview1'].image=pil_to_ui_image(testPhoto.mainImage)
	
	
v = ui.load_view()
v.present('sheet')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
