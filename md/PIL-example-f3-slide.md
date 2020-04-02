---
title: PIL example f3-slide (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'f3-slide'


Modules used in program: 
* `import PIL`
* `import time`
* `import json`
* `import urllib`

## python f3-slide

Python PIL example: f3-slide

```python
#!/usr/bin/python

"""
	aptitude install python-imaging
"""

import urllib
import json
import time
import PIL
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw

url = 'http://www.frequence3.fr/ajax/current'

currentBroadcastId=0

while True:
	try:
		response = urllib.urlopen(url)
	except:
		print("Can not open url %s" % (url))
		time.sleep(5)
		continue

	data = json.loads(response.read())

	if currentBroadcastId == data['current']['broadcastId']:
		time.sleep(2)
	else:
		print(data['current']['artist'] + ' - ' +  data['current']['track'] + ' (' + data['current']['cover'] + ')')
		currentBroadcastId=data['current']['broadcastId']
		
		# Download File
		coverfile = "/tmp/f3/%s - %s.jpg" % (data['current']['artist'], data['current']['track'])
		urllib.urlretrieve (data['current']['cover'], coverfile)
		
		# Resize to 130x130
		cover_basewidth = 130
		cover = Image.open(coverfile)
		wpercent = (cover_basewidth / float(cover.size[0]))
		hsize = int((float(cover.size[1]) * float(wpercent)))
		cover = cover.resize((cover_basewidth, hsize), PIL.Image.ANTIALIAS)
		#cover.save(coverfile)

		# Generate slideshow image 320x240
		slidefile = "/tmp/f3/slide.png"
		slide = Image.open(slidefile)
		
		# Write metadata texte
		draw = ImageDraw.Draw(slide)
		font = ImageFont.truetype("LiberationMono-Regular.ttf",14)
		draw.text((20, 190),data['current']['artist'],(0,0,0),font=font)
		draw.text((20, 210),data['current']['track'],(0,0,0),font=font)
		draw = ImageDraw.Draw(slide)
		
		coverOffset = (150, 25)
		slide.paste(cover, coverOffset)
		slide.save("slide.png")
		
		time.sleep(30)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
