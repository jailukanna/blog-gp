---
title: PIL example image np to image string (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'image np to image string'


Modules used in program: 
* `import cv2`
* `import base64`

## python image np to image string

Python PIL example: image np to image string

```python
import base64
import cv2
from PIL import Image

cap = cv2.VideoCapture(0)
_, img = cap.read()
image_string = cv2.imencode(".png", img)[1].tostring()

output = io.BytesIO()
Image.fromarray(img).save(output, format='PNG')
buf = output.getvalue()
b64 = base64.b64encode(buf)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
