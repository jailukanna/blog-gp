---
title: PIL example test exif (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'test exif'

Functions in program: 
* `def test_exifread():`
* `def test_piexif_use():`
* `def test_piexif_inspect():`
* `def test_piexif():`
* `def test_PIL():`
* `def test_IPTC():`

## python test exif

Python PIL example: test exif

```python
"""
	test_exif.py

	This module tests various third-party libraries for their ability to extract
	metadata from a test JPG file.

	Python 3.4 used on Windows 10.

	This code accompanies the article:
		http://www.theatreofnoise.com/2016/10/how-to-retrieve-photo-metadata-in-python.html
"""

fn = 'path/to/some/file/tester.jpg'

def test_IPTC():
	"""
		tried to install from:
		https://pypi.python.org/pypi/IPTCInfo/

		not Python 3 compatible, so this function is just for show
	"""
	from iptcinfo import iptcinfo
	print( '\n<< Test of iptcinfo >>\n' )

	print(iptcinfo.__dict__)
	iptc = iptcinfo.IPTCInfo(fn)

	image_title = iptc.data.get('object name', '') or iptc.data.get('headline', '')
	image_description = iptc.data.get('caption/abstract', '')
	image_tags = iptc.keywords

def test_PIL():
	"""
		Pillow:
		https://pillow.readthedocs.io/en/3.4.x/index.html
	"""
	from PIL import Image
	from PIL.ExifTags import TAGS
	print( '\n<< Test of PIL>> \n' )

	img = Image.open(fn)
	info = img._getexif()
	for k, v in info.items():
		nice = TAGS.get(k, k)
		print( '%s (%s) = %s' % (nice, k, v) )

def test_piexif():
	"""
		Piexif
		http://piexif.readthedocs.io
	"""
	import piexif
	print( '\n<< Test of Piexif >>' )

	data = piexif.load(fn)
	# returns several dictionaries, plus a byte dump as 'thumbnail'
	for key in ['Exif', '0th', '1st', 'GPS', 'Interop']:
		subdata = data[key]
		print( '\n%s:' % key )
		for k, v in subdata.items():
			print( '%s = %s' % (k, v) )

def test_piexif_inspect():
	"""
		Examines mappings of data codes to names.
	"""
	import piexif
	print( '\n<< Inspect piexif >>\n' )

	info = piexif.ImageIFD.__dict__
	l = ['%s = %s' % (v, k) for k, v in info.items()]
	l.sort()
	for item in l:
		print(item)

def test_piexif_use():
	"""
		Since the above mapping does NOT have a name for the
		comment field, this is how I would need to extract it.
	"""
	import piexif
	print( '\n<< Usage of piexif >>' )
	data = piexif.load(fn)
	exif = data['Exif']
	comment = exif.get(37510, '').decode('UTF-8')
	comment = comment[8:]
	print( comment )

def test_exifread():
	"""
		exifread:
			https://github.com/ianare/exif-py

		Note: does not write tags.
	"""
	import exifread
	print( '\n<< Test of exifread >>\n' )

	with open(fn, 'rb') as f:
		exif = exifread.process_file(f)

	for k in sorted(exif.keys()):
		# omit keys with binary data
		if k not in ['JPEGThumbnail', 'TIFFThumbnail', 'Filename', 'EXIF MakerNote']:
			print( '%s = %s' % (k, exif[k]) )

if __name__ == '__main__':
	test_PIL()
	test_piexif()
	test_exifread()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
