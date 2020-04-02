---
title: pil example picam capture (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'picam capture'


Modules used in program: 
* `import cv2`
* `import picamera.array`
* `import picamera`

## python picam capture

Python pil example: picam capture

```python
import picamera
import picamera.array
import cv2

with picamera.PiCamera() as camera:
	with picamera.array.PiRGBArray(camera) as stream:
		camera.resolution = (320, 240)
		while True:
			camera.capture(stream, 'bgr', use_video_port=True)
			cv2.imshow('frame', stream.array)

			if cv2.waitKey(1) & 0xFF == ord("q"):
				break

			stream.seek(0)
			stream.truncate()
		cv2.destroyAllWindows()


# sudo apt install python3-picamera
# python3 picam_capture.py 

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
