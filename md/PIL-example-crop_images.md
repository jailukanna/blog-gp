---
title: PIL example crop images (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'crop images'


Modules used in program: 
* `import glob`
* `import sys`
* `import os`

## python crop images

Python PIL example: crop images

```python
'''
Script to crop dead space in images and save out coordinates in the file name.  Requires PIL to be installed:
http://www.pythonware.com/products/pil/
usage:
python crop_images.py path/to/input/dir path/to/output/dir
'''
from PIL import Image
from PIL import ImageOps
import os
import sys
import glob

if len(sys.argv) < 3:
  print("Not enough arguments. You need to supply a input and output directory")
	exit(1)

inputDirectory = sys.argv[1]
outputDirectory = sys.argv[2]
processedFiles = 0

fileList = os.listdir(inputDirectory)
for fileName in fileList:
	if fileName.endswith(".png"):
		# load the image
		inputImage = Image.open(inputDirectory + "/" + fileName)
		inputImage.load()
		# setup an alpha image to create a boundbox from
		alpha = inputImage.split()[-1]
		alphaImage = Image.merge("RGB", (alpha, alpha, alpha))
		bounds = alphaImage.getbbox()
		# crop and save the image
		cropped = inputImage.crop(bounds)
		outputSize = "(" + str(bounds[0]) + "," + str(bounds[1]) + "," + str(bounds[2] - bounds[0]) + "," + str(bounds[3] - bounds[1]) + ")"
		outputFilename = fileName.replace(".png", outputSize + ".png")
		cropped.save(outputDirectory + "/" + outputFilename)
		print("saving: " + outputDirectory + "/" + outputFilename)
		processedFiles += 1

if processedFiles == 0:
	print("No images were processed")
else:
	print("Completed processing " + str(processedFiles) + " images")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
