---
title: pil example label detection (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'label detection'


Modules used in program: 
* `import PIL as Pillow`
* `import os, sys, urllib #, MySQLdb`

## python label detection

Python pil example: label detection

```python
import os, sys, urllib #, MySQLdb
import PIL as Pillow
from PIL import Image
from PIL import ImageChops

# # database magic
# db = MySQLdb.connect(host="", user="", passwd="", db="")
# c = db.cursor()
# c.execute("""SELECT statement to get images""")
# myresults = c.fetchall()
# for data in myresults:
#	doc_id = str(data[1])
#	if data[5] is not None:
#		outfile = open(doc_id + "-" + data[3], 'wb')
#		outfile = write(data[5])
#		outfile.close()
#		print("Wrote %s" % doc_id + "-" + data[3])
#	else
#		print("No data for %s" % doc_id + data[3])


# # difference
# diff = ImageChops.difference(im, im2)
# print(diff)

# if ImageChops.difference(im, im2) == 0
#	return 7

#read image
im = Image.open("al-maf.jpg")

print(im.format, im.size, im.mode)
filesize = os.path.getsize("al-maf.jpg")
filesize = int(filesize)

if filesize / 1000000 >= 4:
	# resize image (need option for 4MB or over)
	basewidth = 4000
	wpercent = (basewidth / float(im.size[0]))
	hsize = int((float(im.size[1]) * float(wpercent)))
	im2 = im.resize((basewidth, hsize), Image.ANTIALIAS)
	im.save("trainyard2.jpg")
	print("image has been resized")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
