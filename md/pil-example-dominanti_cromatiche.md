---
title: pil example dominanti cromatiche (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'dominanti cromatiche'

Functions in program: 
* `def distanceBetweenColors(color1, color2):`
* `def catchFilesAndFolders(path, toJump):`

Modules used in program: 
* `import sys`
* `import shutil`
* `import math`
* `import os`

## python dominanti cromatiche

Python pil example: dominanti cromatiche

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

#################################################
# Organizing images by dominant colors with PIL #
#################################################

### Modules
from PIL import Image, ImageStat
import os
import math
import shutil
import sys
from operator import itemgetter


### Functions
def catchFilesAndFolders(path, toJump):
	items = [x for x in os.listdir(path) if x not in toJump]
	return items

def distanceBetweenColors(color1, color2):
	x1 = color1[0]
	y1 = color1[1]
	z1 = color1[2]

	x2 = color2[0]
	y2 = color2[1]
	z2 = color2[2]

	distance = math.sqrt((x2-x1)**2 + (y2-y1)**2 + (z2-z1)**2)
	return distance


### Variables
input_folder = 'input'
output_folder = 'output'

benchmark_colors = {
		"red light":  [ 255, 0, 0],
		"red avg": [ 170, 0, 0],
		"red dark": [ 85, 0, 0],

		"green light": [ 0, 255, 0],
		"green avg": [ 0, 170, 0],
		"green dark": [ 0, 85, 0],

		"blue light": [ 0, 0, 255],
		"blue avg": [ 0, 0, 170],
		"blue dark": [ 0, 0, 85],

		"yellow light": [ 255, 255, 0],
		"yellow avg": [ 170, 170, 0],
		"yellow dark": [ 85, 85, 0],

		"cyan light": [ 0, 255, 255],
		"cyan avg": [ 0, 170, 170],
		"cyan dark": [ 0, 85, 85],

		"violet light": [ 255, 0, 255],
		"violet avg": [ 170, 0, 170],
		"violet dark": [ 85, 0, 85],

		"white": [ 255, 255, 255],
		"black": [ 0, 0, 0],
}

### Instructions
# Checking output folders
for each_key, each_value in benchmark_colors.items():
	toVerify = output_folder + os.sep + each_key

	if os.path.exists(toVerify) == False:
		os.mkdir(toVerify)

# Iterating over images
images_path = catchFilesAndFolders(input_folder, ['.DS_Store'])
for each_image_path in images_path:
	print(each_image_path)

	# Opening image file with PIL Image
	image = Image.open(input_folder + os.sep + each_image_path)

	median = ImageStat.Stat(image).median
	print(median)

	distances = []
	for each_key, each_value in benchmark_colors.items():
		distance = distanceBetweenColors(each_value, median)
		distances.append((distance, each_key))

	sorted_distances = sorted(distances, key=itemgetter(0), reverse = False)
	whatColor = sorted_distances[0][1]
	print(whatColor)
	print
	
	# Paths
	src_path = input_folder + os.sep + each_image_path
	new_path = output_folder + os.sep + whatColor + os.sep + each_image_path

	shutil.copy2(src_path, new_path)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
