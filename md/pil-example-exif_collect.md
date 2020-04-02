---
title: pil example exif collect (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'exif collect'

Functions in program: 
* `def check_image_with_pil(path):`

Modules used in program: 
* `import sys`
* `import shutil`
* `import os`
* `import exifread`

## python exif collect

Python pil example: exif collect

```python
from PIL import Image
import exifread
from datetime import datetime as dt
import os
import shutil
import sys

###
# detect if the file is an image
##
def check_image_with_pil(path):
	try:
		Image.open(path)
	except IOError:
		return False
	return True

if not os.path.exists("/Users/lic/exifclean"):
	os.mkdir("/Users/lic/test/exifclean")

src_path = sys.argv[1]
img_files = [f for f in os.listdir(src_path) if os.path.isfile(os.path.join(src_path, f))]
print(len(img_files))
clean_target_dir_path = ""
for fname in img_files:
	img_path = "%s/%s" % (src_path, fname)
	if check_image_with_pil(img_path):
		f = open(img_path, "rb")
		tags = exifread.process_file(f, details=False)
		meta_keys = tags.keys()
		datetime_meta_key = "Image DateTime"
		#for tag in tags.keys():
			#if tag not in ('JPEGThumbnail', 'TIFFThumbnail', 'Filename', 'EXIF MakerNote'):
		if not "Image DateTime" in meta_keys:
			if "EXIF DateTimeOriginal" in meta_keys:
				datetime_meta_key = "EXIF DateTimeOriginal"
			else:
				# no datetime metadata
				datetime_meta_key = None
		if datetime_meta_key is not None:
			img_datetime = dt.strptime("%s" % tags[datetime_meta_key], "%Y:%m:%d %H:%M:%S")
			img_date_str = img_datetime.strftime("%Y")
			print("The file [%s] -- Key: %s, value is %s" % (img_path, datetime_meta_key, img_date_str))
			clean_target_dir_path = "/Users/lic/exifclean/%s" % img_date_str
		else:
			clean_target_dir_path = "/Users/lic/exifclean/nometa"
			print("The file [%s] does not contain date time meta-info [%s]" % (img_path, datetime_meta_key))
	else:
		clean_target_dir_path = "/Users/lic/exifclean/waiting"
		print("The file [%s] is not an available image." % img_path)

	if not os.path.exists(clean_target_dir_path):
		os.mkdir(clean_target_dir_path)
	#copy this image into the date time tagged directory
	shutil.copy2(img_path, clean_target_dir_path)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
