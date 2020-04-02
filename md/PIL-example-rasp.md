---
title: PIL example rasp (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'rasp'


Modules used in program: 
* `import json`
* `import zbar`
* `import requests`
* `import picamera`
* `import time`
* `import io`

## python rasp

Python PIL example: rasp

```python
import io
import time
import picamera
import requests
from PIL import Image
import zbar
import json

aws = "http://******.us-west-2.compute.amazonaws.com:8080/auth"

count = 0
while True:
	count = count + 1
	if count == 5:
		count = 0
	stream = io.BytesIO()
	with picamera.PiCamera() as camera:
		camera.start_preview()
		time.sleep(0.5)
		camera.capture('image'+str(count)+'.jpg')
		camera.capture(stream, format='jpeg')
		
	stream.seek(0)
	pil = Image.open(stream)

	scanner = zbar.ImageScanner()
	scanner.parse_config('enable')
	
	pil = pil.convert('L')
	width, height = pil.size
	raw = pil.tostring()

	# wrap image data
	image = zbar.Image(width, height, 'Y800', raw)

	# scan the image for barcodes
	scanner.scan(image)
	
	print('scanning')
	# extract results
	for symbol in image:
		# do something useful with results
		json = {"deviceID": symbol.data[1:-1]}
		r = requests.post(aws, data=json)
		print((r.status_code))
		print((r.text))
		
	# clean up
	del(image)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
