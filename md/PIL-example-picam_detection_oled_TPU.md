---
title: PIL example picam detection oled TPU (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'picam detection oled TPU'

Functions in program: 
* `def detect_from_camera():`

Modules used in program: 
* `import picamera.array`
* `import picamera`
* `import time`

## python picam detection oled TPU

Python PIL example: picam detection oled TPU

```python
import time
import picamera
import picamera.array

from PIL import Image
from luma.core.interface.serial import i2c, spi
from luma.core.render import canvas
from luma.oled.device import ssd1306, ssd1309, ssd1325, ssd1331, sh1106

from edgetpu.detection.engine import DetectionEngine

MODEL_NAME = "mobilenet_ssd_v2_coco_quant_postprocess_edgetpu.tflite"

label2string = \
{
	0:   "person",
	1:   "bicycle",
	2:   "car",
	3:   "motorcycle",
	4:   "airplane",
	5:   "bus",
	6:   "train",
	7:   "truck",
	8:   "boat",
	9:   "traffic light",
	10:  "fire hydrant",
	12:  "stop sign",
	13:  "parking meter",
	14:  "bench",
	15:  "bird",
	16:  "cat",
	17:  "dog",
	18:  "horse",
	19:  "sheep",
	20:  "cow",
	21:  "elephant",
	22:  "bear",
	23:  "zebra",
	24:  "giraffe",
	26:  "backpack",
	27:  "umbrella",
	30:  "handbag",
	31:  "tie",
	32:  "suitcase",
	33:  "frisbee",
	34:  "skis",
	35:  "snowboard",
	36:  "sports ball",
	37:  "kite",
	38:  "baseball bat",
	39:  "baseball glove",
	40:  "skateboard",
	41:  "surfboard",
	42:  "tennis racket",
	43:  "bottle",
	45:  "wine glass",
	46:  "cup",
	47:  "fork",
	48:  "knife",
	49:  "spoon",
	50:  "bowl",
	51:  "banana",
	52:  "apple",
	53:  "sandwich",
	54:  "orange",
	55:  "broccoli",
	56:  "carrot",
	57:  "hot dog",
	58:  "pizza",
	59:  "donut",
	60:  "cake",
	61:  "chair",
	62:  "couch",
	63:  "potted plant",
	64:  "bed",
	66:  "dining table",
	69:  "toilet",
	71:  "tv",
	72:  "laptop",
	73:  "mouse",
	74:  "remote",
	75:  "keyboard",
	76:  "cell phone",
	77:  "microwave",
	78:  "oven",
	79:  "toaster",
	80:  "sink",
	81:  "refrigerator",
	83:  "book",
	84:  "clock",
	85:  "vase",
	86:  "scissors",
	87:  "teddy bear",
	88:  "hair drier",
	89:  "toothbrush",
}

def detect_from_camera():
	# Load model and prepare TPU engine
	engine = DetectionEngine(MODEL_NAME)

	# Prepare OLED
	serial = i2c(port=1, address=0x3C)
	device = sh1106(serial)

	with picamera.PiCamera() as camera:
		with picamera.array.PiRGBArray(camera) as stream:
			camera.resolution = (640, 480)
			while True:
				start = time.time()
				# capture image
				camera.capture(stream, 'rgb', use_video_port=True)
				pil_img = Image.fromarray(stream.array)
				pil_img = pil_img.resize((300, 300), Image.NEAREST)
	
				# Run inference
				ans = engine.DetectWithImage(pil_img, threshold=0.5, keep_aspect_ratio=True, relative_coord=True, top_k=10)

				# Retrieve results
				with canvas(device) as draw:
					if ans:
						for obj in ans:
							print(('-----------------------------------------'))
							print('label = ', label2string[obj.label_id])
							print(('score = ', obj.score))
							box = obj.bounding_box.flatten().tolist()
							print(('box = ', box))

							x0 = int(box[0] * 128)
							y0 = int(box[1] * 64)
							x1 = int(box[2] * 128)
							y1 = int(box[3] * 64)
						
							draw.rectangle((x0, y0, x1, y1), outline="white", fill=None)
							draw.text((x0, y0), label2string[obj.label_id], fill="white")


				print('inference time = ', engine.get_inference_time() , '[msec]')
				elapsed_time = time.time() - start
				print('total time = ', elapsed_time * 1000 , '[msec] (', 1 / elapsed_time, ' fps)')

				stream.seek(0)
				stream.truncate()




if __name__ == '__main__':
	detect_from_camera()


'''
for Raspberry Pi Zero W

Connect OLED(SH1106) i2c to 3(SDA) and 5(SCL) pin

```
sudo apt-get install i2c-tools
sudo raspi-config
# enable camera and i2c
i2cdetect -y 1


wget https://github.com/google-coral/edgetpu-platforms/releases/download/v1.9.2/edgetpu_api_1.9.2.tar.gz
tar xzf edgetpu_api_1.9.2.tar.gz
cd edgetpu_api/
./install.sh

wget https://dl.google.com/coral/canned_models/mobilenet_ssd_v2_coco_quant_postprocess_edgetpu.tflite

sudo apt install python3-picamera

sudo apt-get install  libfreetype6-dev libjpeg-dev build-essential
sudo  pip3 install  luma.oled

```

'''

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
