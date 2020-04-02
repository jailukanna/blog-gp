---
title: PIL example android icon generator (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'android icon generator'


Modules used in program: 
* `import sys`
* `import getopt`
* `import os`

## python android icon generator

Python PIL example: android icon generator

```python
# Uses PIL library for resizing 
# http://www.pythonware.com/products/pil/

# resize.py arg1 arg2 
# arg1 : src image name
# arg2 : dir path of src image

import os
import getopt
import sys
from PIL import Image

image = sys.argv[1] 
directory = sys.argv[2]

out_img = 'ic_launcher.png'

print('image name : ' + image)
print('dir   name : ' + directory)


pathArray = ['res\\drawable-ldpi','res\\drawable-mdpi','res\\drawable-hdpi','res\\drawable-xhdpi','res\\drawable-xxhdpi']
sizeArray = [36,48,72,96,144]

print('## making directory structure')

for path in pathArray:
	os.makedirs(directory + '\\' + path)

print('## directory structure done')

# Open the image file.
img = Image.open(os.path.join(directory, image))
 
# Resize it.
for i in range(0,5):
	img1 = img.resize((sizeArray[i],sizeArray[i]), Image.ANTIALIAS) 
	img1.save(os.path.join(directory, pathArray[i] +'\\'+ out_img))
	print('# image drawable ' + str(sizeArray[i]) +' x '+ str(sizeArray[i]))


print(' --------------- done --------------- ')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
