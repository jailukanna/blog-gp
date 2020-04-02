---
title: PIL example exif data (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'exif data'

Functions in program: 
* `def resizeImg(filename):	`

Modules used in program: 
* `import PIL`
* `import json`
* `import pprint`
* `import os`
* `import gpsphoto`
* `import exifread`

## python exif data

Python PIL example: exif data

```python
# import piexif
import exifread
import gpsphoto
import os
import pprint
import json
import PIL
from PIL import Image

cwd = os.getcwd() #get current path as string

images_data = []

images_directory = os.path.join(cwd, '../images') #os.path.join allows this to work on windows as well for \

for filename in os.listdir(images_directory): 
	image_data = {
		"filename": filename
	}
	image_path = os.path.join(images_directory, filename)

	resizeImg(image_path)
	
	file = open(image_path, 'rb')
	tags = exifread.process_file(file, details=False)

	# import pdb; pdb.set_trace()
	# pprint.pprint(tags)

	# print(image_path)
	data = gpsphoto.getGPSData(image_path)
	# pprint.pprint(data)
	if 'Latitude' in data and 'Longitude' in data:
		image_data['lat'] = data['Latitude']
		image_data['long'] = data['Longitude']
		image_data['alt'] = data['Altitude']

	images_data.append(image_data)

	# exix_dict = piexif.load(image_path)
	# print(exix_dict)

pprint.pprint(images_data)
with open('exif.json', 'w') as outfile:
	json.dump(images_data, outfile);


def resizeImg(filename):	
	basewidth = 300
	img = Image.open(filename)
	wpercent = (basewidth/float(img.size[0]))
	hsize = int((float(img.size[1])*float(wpercent)))
	img = img.resize((basewidth,hsize), PIL.Image.ANTIALIAS)
	newImgName = os.path.join(images_directory, '/resized', '/rs_', filename)
	img.save(newImgName) 


# def getcords(d, m, s, ind):
# 	# Calculate the total number of seconds, 
# 	# 43'41" = (43*60 + 41) = 2621 seconds.

# 	sec = float((m * 60) + s)
# 	# The fractional part is total number of 
# 	# seconds divided by 3600. 2621 / 3600 = ~0.728056

# 	frac = float(sec / 3600)
# 	# Add fractional degrees to whole degrees 
# 	# to produce the final result: 87 + 0.728056 = 87.728056

# 	deg = float(d + frac)
# 	# If it is a West or S longitude coordinate, negate the result.
# 	if ind == 'W':
# 		deg = deg * -1

# 	if ind == 'S':
# 		deg = deg * -1

# 	return float(deg)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
