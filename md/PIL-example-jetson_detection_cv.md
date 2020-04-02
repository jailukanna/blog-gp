---
title: PIL example jetson detection cv (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'jetson detection cv'

Functions in program: 
* `def detect_from_camera():`
* `def detect_from_image():`
* `def cv2pil(image_cv):`

Modules used in program: 
* `import cv2`
* `import time`

## python jetson detection cv

Python PIL example: jetson detection cv

```python
import time
# import picamera
# import picamera.array
import cv2
from PIL import Image
from edgetpu.detection.engine import DetectionEngine

MODEL_NAME = "mobilenet_ssd_v2_coco_quant_postprocess_edgetpu.tflite"

def cv2pil(image_cv):
	image_cv = cv2.cvtColor(image_cv, cv2.COLOR_BGR2RGB)
	image_pil = Image.fromarray(image_cv)
	image_pil = image_pil.convert('RGB')
	return image_pil

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

def detect_from_image():
	# Load model and prepare TPU engine
	engine = DetectionEngine(MODEL_NAME)

	# prepara input image
	img_org = cv2.imread('input.jpg')
	#	cv2.imshow('image', img)
	pil_img = cv2pil(cv2.resize(img_org, (300, 300)))

	# Run inference
	ans = engine.DetectWithImage(pil_img, threshold=0.5, keep_aspect_ratio=True, relative_coord=True, top_k=10)

	# Retrieve results
	if ans:
		for obj in ans:
			print(('-----------------------------------------'))
			print('label = ', label2string[obj.label_id])
			print(('score = ', obj.score))
			box = obj.bounding_box.flatten().tolist()
			print(('box = ', box))

			x0 = int(box[0] * img_org.shape[1])
			y0 = int(box[1] * img_org.shape[0])
			x1 = int(box[2] * img_org.shape[1])
			y1 = int(box[3] * img_org.shape[0])
			cv2.rectangle(img_org, (x0, y0), (x1, y1), (255, 0, 0), 2)
			cv2.rectangle(img_org, (x0, y0), (x0 + 100, y0 - 30), (255, 0, 0), -1)
			cv2.putText(img_org,
					str(label2string[obj.label_id]),
					(x0, y0),
					cv2.FONT_HERSHEY_SIMPLEX,
					1,
					(255, 255, 255),
					2)

		cv2.imshow('image', img_org)
		cv2.waitKey(0)
		cv2.destroyAllWindows()

def detect_from_camera():
	# Load model and prepare TPU engine
	engine = DetectionEngine(MODEL_NAME)

	cap = cv2.VideoCapture(0)

	while True:
		start = time.time()
		# capture image
		ret, img_org = cap.read()
#		cv2.imshow('image', img_org)
		key = cv2.waitKey(1)
		if key == 27: # ESC
			break
		
		pil_img = cv2pil(cv2.resize(img_org, (300, 300)))

		# Run inference
		ans = engine.DetectWithImage(pil_img, threshold=0.5, keep_aspect_ratio=True, relative_coord=True, top_k=10)

		# Retrieve results
		if ans:
			for obj in ans:
				print(('-----------------------------------------'))
				print('label = ', label2string[obj.label_id])
				print(('score = ', obj.score))
				box = obj.bounding_box.flatten().tolist()
				print(('box = ', box))

				x0 = int(box[0] * img_org.shape[1])
				y0 = int(box[1] * img_org.shape[0])
				x1 = int(box[2] * img_org.shape[1])
				y1 = int(box[3] * img_org.shape[0])
				cv2.rectangle(img_org, (x0, y0), (x1, y1), (255, 0, 0), 2)
				cv2.rectangle(img_org, (x0, y0), (x0 + 100, y0 - 30), (255, 0, 0), -1)
				cv2.putText(img_org,
						str(label2string[obj.label_id]),
						(x0, y0),
						cv2.FONT_HERSHEY_SIMPLEX,
						1,
						(255, 255, 255),
						2)

		# Draw the result
		cv2.imshow('image', img_org)
		if cv2.waitKey(1) & 0xFF == ord("q"):
			break
		
		print('inference time = ', engine.get_inference_time() , '[msec]')
		elapsed_time = time.time() - start
		print('total time = ', elapsed_time * 1000 , '[msec] (', 1 / elapsed_time, ' fps)')

	cv2.destroyAllWindows()


if __name__ == '__main__':
	detect_from_camera()
	# detect_from_image()



'''
for Jetson Nano

sudo apt-get install libjpeg-dev
pip3 install pillow

cd ~/
wget https://dl.google.com/coral/edgetpu_api/edgetpu_api_latest.tar.gz -O edgetpu_api.tar.gz --trust-server-names
tar xzf edgetpu_api.tar.gz
cd edgetpu_api
bash ./install.sh
sudo ln -s /usr/local/lib/python3.6/dist-packages/edgetpu/swig/_edgetpu_cpp_wrapper.cpython-35m-aarch64-linux-gnu.so  /usr/local/lib/python3.6/dist-packages/edgetpu/swig/_edgetpu_cpp_wrapper.cpython-36m-aarch64-linux-gnu.so

python3 cv_detection.py

https://dl.google.com/coral/canned_models/mobilenet_ssd_v2_coco_quant_postprocess_edgetpu.tflite
'''


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
