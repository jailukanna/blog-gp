---
title: PIL example python-roboq (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'python-roboq'


Modules used in program: 
* `import json`
* `import zbar`
* `import requests`
* `import picamera`
* `import time`
* `import io `

## python python-roboq

Python PIL example: python-roboq

```python
#COMMANDOS NECESSÀRIOS:

#sudo apt-get update
#sudo apt-get install python-picamera python3-picamera
#pip install picamera
#pip install requests
#pip install image
#pip install hcsr04sensor



import io 
import time
import picamera
import requests
from PIL import Image
import zbar
import json
from hcsr04sensor import sensor

aws = "http://ec2-52-43-169-138.us-west-2.compute.amazonaws.com:8080/auth"


count = 0
TRIG = 20
ECHO = 26
while True:
	value = sensor.Measurement(TRIG, ECHO)
	raw_measurement = value.raw_distance()
	metric_distance = value.distance_metric(raw_measurement)
	print("The Distance = {} centimeters".format(metric_distance))

	if (metric_distance <= 24):	
		count = count + 1
		if count == 5:
			count = 0
		# Create the in-memory stream
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
		image = zbar.Image(width, height, 'Y800', raw)
		scanner.scan(image)
		
		print('scanning')
		# extract results
		for symbol in image:
			# do something useful with results
			print(symbol.data	)
			json = {"deviceID": symbol.data}
			 = requests.post(aws, data=json)
			
			print((r.status_code))
			print((r.text))

			
			
		
		# clean up
		del(image)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
