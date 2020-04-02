---
title: PIL example sort backgrounds (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'sort backgrounds'


Modules used in program: 
* `import shutil`
* `import os.path as path`
* `import os`

## python sort backgrounds

Python PIL example: sort backgrounds

```python
#! /usr/bin/python2.7
"""
Requires:
    Python Imaging Library http://www.pythonware.com/products/pil/
		Version: 1.1.7 for 2.7

Information:
	Python: 2.7.5
	Version: 1.1.0
	Author: Michael Borden <taksuyu@gmail.com>

License: 
/* This program is free software. It comes without any warranty, to
     * the extent permitted by applicable law. You can redistribute it
     * and/or modify it under the terms of the Do What The Fuck You Want
     * To Public License, Version 2, as published by Sam Hocevar. See
     * http://www.wtfpl.net/ for more details. */

Notes:
	Currently takes no arguments but will sort every valid image into 
		folders based on the dimensions of the image itself.
"""
from PIL import Image
import os
import os.path as path
import shutil

statistics = dict()
input_dir = "oops"

for each in os.listdir(os.getcwd()):
	if path.isdir(each) or each.endswith(".py"):
		continue
	elif path.isfile(each):
		try:
			with open(each, 'rb') as f:
				img = Image.open(f)
				input_dir = "{0}x{1}".format(img.size[0], img.size[1])
		except:
			print("%s isn't an image supported by PIL" % each)
			continue

		dir_exists = False

		#Check to see if the directory for the image size exists
		if path.exists(input_dir) and path.isdir(input_dir):
			dir_exists = True
			#Check if the file already exists in that folder
			if path.exists("%s/%s" % (input_dir, each)):
				print("%s already exists in %s!" % (each, input_dir))
				continue
		else:
			print("%s doesn't exist; Creating" % input_dir)
			#Create the directory
			os.mkdir(str(input_dir))
		
		#Move the files
		shutil.move(each, "%s/%s" % (input_dir, each))
		
		#Check if key is in the dictionary
		if input_dir in statistics.keys():
			statistics[input_dir] += 1
		else:
			statistics[input_dir] = 1

#Reporting :D
print("\nResults:")
for key_pair in statistics.items():
	print("%s: %s" % (key_pair[0], key_pair[1]))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
