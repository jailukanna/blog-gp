---
title: PIL example opencv-qrcode (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'opencv-qrcode'


Modules used in program: 
* `import zbar`
* `import cv2`

## python opencv-qrcode

Python PIL example: opencv-qrcode

```python
#installed zbar with difficulty, main steps taken from https://github.com/NaturalHistoryMuseum/gouda
#what worked for me (executed from anaconda command prompt)
#conda update --all
#conda update --all
#python -m pip install --upgrade pip
#python <Anaconda dir>\Scripts\pywin32_postinstall.py -install
#python -m pip install pathlib
#python -m pip install numpy
#python -m pip install Pillow
#conda install -c menpo opencv
#python -m pip install https://github.com/NaturalHistoryMuseum/zbar-python-patched/releases/download/v0.10/zbar-0.10-cp27-none-win32.whl

import cv2
import zbar
from PIL import Image

cv2.namedWindow("#iothack15")
cap = cv2.VideoCapture(0)

scanner = zbar.ImageScanner()
scanner.parse_config('enable')

# Capture frames from the camera
while True:
    ret, output = cap.read()
    if not ret:
	  continue
    gray = cv2.cvtColor(output, cv2.COLOR_BGR2GRAY, dstCn=0)
    pil = Image.fromarray(gray)
    width, height = pil.size
    raw = pil.tobytes()
    image = zbar.Image(width, height, 'Y800', raw)
    scanner.scan(image)
	
    for symbol in image:
        print('decoded', symbol.type, 'symbol', '"%s"' % symbol.data)

    cv2.imshow("#iothack15", output)

    # clear stream for next frame
    #rawCapture.truncate(0)

    # Wait for the magic key
    keypress = cv2.waitKey(1) & 0xFF
    if keypress == ord('q'):
    	break

# When everything is done, release the capture
cap.release()
cv2.destroyAllWindows()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
