---
title: PIL example make thumbs (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'make thumbs'

Functions in program: 
* `def rescale(data, width, height, force=True):`

Modules used in program: 
* `import os`
* `import paramiko as ssh_c`
* `import Image as pil`
* `import urllib2`

## python make thumbs

Python PIL example: make thumbs

```python
#!/usr/bin/python
"""
This file takes a url of an image, resizes it, and SFTPs the new
image to a new location.  The image URLs are stored in a MySQL db in 
my case, but YMMV
"""
import urllib2
import Image as pil
import paramiko as ssh_c
import os

def rescale(data, width, height, force=True):
	from cStringIO import StringIO

	"""Rescale the given image, optionally cropping it to make sure the result image has the specified width and height."""
	max_width = width
	max_height = height

	input_file = StringIO(data)
	img = pil.open(input_file)
	if not force:
		img.thumbnail((max_width, max_height), pil.ANTIALIAS)
	else:
		src_width, src_height = img.size
		src_ratio = int(src_width) / float(src_height)
		dst_width, dst_height = max_width, max_height
		dst_ratio = int(dst_width) / float(dst_height)
																						
		if dst_ratio < src_ratio:
			crop_height = src_height
			crop_width = crop_height * dst_ratio
			x_offset = int(src_width - crop_width) / 2
			y_offset = 0
		else:
			crop_width = src_width
			crop_height = crop_width / dst_ratio
			x_offset = 0
			y_offset = int(src_height - crop_height) / 3
		img = img.crop((x_offset, y_offset, x_offset+int(crop_width), y_offset+int(crop_height)))
		img = img.resize((dst_width, dst_height), pil.ANTIALIAS)
		input_file.close()

	return img



url="http://myimage.example.com/myimage.jpg"
try:
	img = urllib2.urlopen(url).read()
except Exception, err:
	print("fail:bad-link")
		
try:
	t_img = rescale(img,200,200)
except Exception, err:
	print("fail:no-resize")
	
img_name = "saveme_simple.jpg"

try:
	t_img.save(img_name,'JPEG')
except Exception, err:
	os.remove(img_name)
	print("fail:no-save")

#sftp to remote site and save
ssh = ssh_c.SSHClient()
ssh.set_missing_host_key_policy(ssh_c.AutoAddPolicy())
ssh.connect('my.example.com', username='ssh-username', password='ssh-password')
	sftp = ssh.open_sftp()
sftp.put(img_name,'/some/dir/loc/'+img_name)
os.remove(img_name)
sftp.close()
ssh.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
