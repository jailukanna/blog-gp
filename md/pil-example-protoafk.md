---
title: pil example protoafk (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'protoafk'


Modules used in program: 
* `import time`
* `import numpy as np`
* `import cv2`
* `import subprocess`

## python protoafk

Python pil example: protoafk

```python
import subprocess
import cv2
from PIL import Image
import numpy as np
import time


print("Start")

windows = subprocess.run("""for i in `xprop -root|grep "_NET_CLIENT_LIST_STACKING(WINDOW): window id" |tr '#' ','|tr ',' '\n'| grep 0x`;do xwininfo -id $i|grep "Window id" ;done""", stdout = subprocess.PIPE, shell=True)


windowList = windows.stdout.decode("utf-8").split("\n")

idList = []

for window in windowList:

	if "RuneScape" in window:

		words = window.split(" ")
		
		idList.append(words[3])


correctID = 0

width = 0

for id in idList:
	xwininfo= subprocess.run("xwininfo -metric -id "+id, stdout=subprocess.PIPE, shell=True)
	
	windowInfo = xwininfo.stdout.decode("utf-8")

	before,during,after = windowInfo.partition("Width: ")

	if width < int(after.split(" ")[0]):
		width = int(after.split(" ")[0])
		correctID = id

print("ID selected: "+correctID)

start = time.time()
screenshot = subprocess.run("import -window "+correctID+" png:-", stdout=subprocess.PIPE, shell=True).stdout
end = time.time()
elapsed = end-start
print("screenshot taken in "+str(elapsed)+"s")

nparr = np.fromstring(screenshot, np.uint8)

img_rgb = cv2.imdecode(nparr, cv2.IMREAD_COLOR)
img_gray = cv2.cvtColor(img_rgb, cv2.COLOR_BGR2GRAY)


template = cv2.imread("stats.png", 0)
w, h = template.shape[::-1]

res = cv2.matchTemplate(img_gray, template, cv2.TM_CCOEFF_NORMED)
threshold = 0.65
loc = np.where(res >= threshold)

print("template match")

coords = cv2.minMaxLoc(res)[-1]
img_crop = img_rgb[coords[1]:coords[1]+h, coords[0]:coords[0]+w]

hp_crop = img_crop[7:7+16, 35:35+85]

img_crop_gray = cv2.cvtColor(hp_crop, cv2.COLOR_BGR2GRAY)


pil_crop = Image.fromarray(img_crop_gray)
factor = 3
pil_crop_rescale = pil_crop.resize((pil_crop.width * factor, pil_crop.height * factor))

print("Image manip")

pil_crop_rescale.save("tempfile.png", "PNG")

print("Temp file saved")

text = subprocess.run("tesseract tempfile.png stdout -l rune", stdout=subprocess.PIPE, shell=True).stdout
text = text.decode("utf-8")

print(text)
pil_crop_rescale.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
