---
title: pil example image merger (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'image merger'

Functions in program: 
* `def merge_images(file1, file2):`

## python image merger

Python pil example: image merger

```python
from os import listdir
from os.path import isfile, join
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw

game_files = "PATH_FOR_First_Images"
dlc_files = "PATH_FOR_Second_Images"
out_put_files = "OUTPUT_PATH"

gfiles = [f for f in listdir(game_files) if isfile(join(game_files, f))]
dlcfiles = [f for f in listdir(dlc_files) if isfile(join(dlc_files, f))]

def merge_images(file1, file2):
	"""Merge two images into one, displayed side by side
	:param file1: path to first image file
	:param file2: path to second image file
	:return: the merged Image object
	"""
	image1 = Image.open(file1)
	image2 = Image.open(file2)
	
	game_cdkey_text = Image.new('RGB', (100, 55))
	draw = ImageDraw.Draw(game_cdkey_text)
	# font = ImageFont.truetype(<font-file>, <font-size>)
	font = ImageFont.truetype("georgia.ttf", 16)
	# draw.text((x, y),"Sample Text",(r,g,b))
	draw.text((0, 0),"Game CDkey:",(255,255,255),font=font)
	
	dlc_cdkey_text = Image.new('RGB', (100, 55))
	draw = ImageDraw.Draw(dlc_cdkey_text)
	# font = ImageFont.truetype(<font-file>, <font-size>)
	font = ImageFont.truetype("georgia.ttf", 16)
	# draw.text((x, y),"Sample Text",(r,g,b))
	draw.text((0, 0),"DLC CDkey:",(255,255,255),font=font)

	(width1, height1) = image1.size
	(width2, height2) = image2.size

	result_width = max(width1,width2)+100
	result_height = height1+height2+5

	result = Image.new('RGB', (result_width, result_height))
	result.paste(im=game_cdkey_text, box=(0, 35))
	result.paste(im=image1, box=(100, 0))
	result.paste(im=dlc_cdkey_text, box=(0, height1+35))
	result.paste(im=image2, box=(100, height1+5))
	return result

for i in range(0,50):
	out = merge_images(join(game_files,gfiles[i]), join(dlc_files,dlcfiles[i]))
	out.save('%s/output_%s.jpg' % (out_put_files, i))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
