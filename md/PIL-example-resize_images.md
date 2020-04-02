---
title: PIL example resize images (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'resize images'


Modules used in program: 
* `import os, os.path`
* `import sys`
* `import PIL`
* `import datetime`

## python resize images

Python PIL example: resize images

```python
from PIL import Image, ExifTags
import datetime
import PIL
import sys
import os, os.path

imgs = list()

if len(sys.argv) > 1:
	path =  str(sys.argv[1]) if str(sys.argv[1]) and  str(sys.argv[1]) != '' else "./"
else:
	path = "./"

valid_images = [".jpg",".gif",".png"]
basewidth = 1280
limit_size_bytes = 500000 # = 500 kb
files_in_folder = len(os.listdir(path))
image_files_in_folder = 0
files_resized = 0
files_with_error = 0

print('\nResize Process Start (%s)\n' % datetime.datetime.now())
for f in os.listdir(path):
	ext = os.path.splitext(f)[1]
	if ext.lower() not in valid_images:
		continue
	image_files_in_folder+=1	
	bytes_size = os.stat(os.path.join(path,f)).st_size	
	if bytes_size > limit_size_bytes:
		img = Image.open(os.path.join(path,f))
		wpercent = (basewidth/float(img.size[0]))
		hsize = int((float(img.size[1])*float(wpercent)))

		meta_data_ok = False
		try:
			img._getexif().items()
			meta_data_ok = True
		except:
			pass

		#import pdb; pdb.set_trace()	
		#try:
		if meta_data_ok:			

			for orientation in ExifTags.TAGS.keys() : 
				if ExifTags.TAGS[orientation]=='Orientation' : 
					break 			

			exif=dict(img._getexif().items())

			try:

				if   exif[orientation] == 3 : 
				    img=img.rotate(180, expand=True)
				elif exif[orientation] == 6 : 
				    img=img.rotate(270, expand=True)
				elif exif[orientation] == 8 : 
				    img=img.rotate(90, expand=True)
			except:
				pass

		img.thumbnail((basewidth,hsize), PIL.Image.ANTIALIAS)
		img.save(os.path.join(path,f))
		img.close()
		files_resized+=1
		#except Exception as e:
			#print('\n Error With Image (%s) -> %s' % (os.path.join(path,f), e.message))
			#files_with_error+=1
			#pass	

print('\nResize Process End At %s , Files In Folder %s, Images In Folder %s, Resized Images %s \n' % (datetime.datetime.now(), files_in_folder, image_files_in_folder, files_resized))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
