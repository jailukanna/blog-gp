---
title: PIL example pi detection pil (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'pi detection pil'

Functions in program: 
* `def detect_from_image():`

Modules used in program: 
* `import numpy`
* `import time`

## python pi detection pil

Python PIL example: pi detection pil

```python
import time
from PIL import Image, ImageDraw, ImageFont
import numpy
from edgetpu.detection.engine import DetectionEngine

MODEL_NAME = "detect_edgetpu.tflite"
MODEL_WIDTH = 300
MODEL_HEIGHT = 300

def detect_from_image():
	### Load model and prepare TPU engine
	engine = DetectionEngine(MODEL_NAME)

	### prepara input image
	img_org = Image.open('input.jpg')
	draw = ImageDraw.Draw(img_org)
	img_resized = img_org.resize((MODEL_WIDTH, MODEL_HEIGHT))
	input_tensor = numpy.asarray(img_resized).flatten()
	
	### Run inference
	# ans = engine.DetectWithImage(img_resized, threshold=0.5, keep_aspect_ratio=True, relative_coord=True, top_k=10)
	ans = engine.DetectWithInputTensor(input_tensor, threshold=0.5, top_k=10)

	### Retrieve results
	if ans:
		for obj in ans:
			print('-----------------------------------------')
			print('label = ', obj.label_id)
			print('score = ', obj.score)
			box = obj.bounding_box.flatten().tolist()
			print('box = ', box)

			x0 = int(box[0] * img_org.size[0])
			y0 = int(box[1] * img_org.size[1])
			x1 = int(box[2] * img_org.size[0])
			y1 = int(box[3] * img_org.size[1])
			draw.rectangle((x0, y0, x1, y1), fill=None, outline=(0, 255, 0))
			draw.text((x0, y0), str(obj.label_id), fill=(0, 255, 0))
	img_org.show()

	### Time Measurement
	start = time.time()
	num_measurement = 100
	for i in range(num_measurement):
		engine.RunInference(input_tensor)
	elapsed_time = time.time() - start
	print(("elapsed_time:{0}".format(1000 * elapsed_time / num_measurement) + "[msec]"))

if __name__ == '__main__':
	detect_from_image()



'''
for Raspberry Pi

sudo apt install imagemagick
pip3 install pillow

python3 jetson_detection_cv.py

https://dl.google.com/coral/canned_models/mobilenet_ssd_v2_coco_quant_postprocess_edgetpu.tflite

'''


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
