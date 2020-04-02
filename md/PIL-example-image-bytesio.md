---
title: PIL example image-bytesio (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'image-bytesio'


Modules used in program: 
* `import cv2`

## python image-bytesio

Python PIL example: image-bytesio

```python
from io import BytesIO


import cv2

cap = cv2.VideoCapture('rtsp://admin:1234@192.168.21.152:554/video1')

ret, frame = cap.read()


success, encoded_frame = cv2.imencode('.jpg', frame)
cv2_bytes_frame = BytesIO(encoded_frame)


from PIL import Image


pil_bytes_frame = BytesIO()
Image.fromarray(cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)).save(pil_bytes_frame, 'jpeg')


Image.open(cv2_bytes_frame).show()
Image.open(pil_bytes_frame).show()


cap.release()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
