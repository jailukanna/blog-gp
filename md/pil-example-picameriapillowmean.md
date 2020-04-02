---
title: pil example picameriapillowmean (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'picameriapillowmean'


Modules used in program: 
* `import picamera`
* `import time`
* `import io`

## python picameriapillowmean

Python pil example: picameriapillowmean

```python
import io
import time
import picamera
from PIL import Image
from PIL import ImageStat

stream = io.BytesIO()
with picamera.PiCamera() as camera:
    camera.start_preview()
    time.sleep(2)
    camera.capture(stream, format='jpeg')
stream.seek(0)
image = Image.open(stream).convert('L')
stat = ImageStat.Stat(image)
print("Brighness of image: ", stat.mean[0])


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
