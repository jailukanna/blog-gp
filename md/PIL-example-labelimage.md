---
title: PIL example labelimage (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'labelimage'


Modules used in program: 
* `import io`

## python labelimage

Python PIL example: labelimage

```python
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont
from urllib.request import urlopen
import io

"""
Downloading the image and passing it to PIL as a stream
"""
file = urlopen("http://imgsv.imaging.nikon.com/lineup/lens/zoom/normalzoom/af-s_dx_18-140mmf_35-56g_ed_vr/img/sample/sample1_l.jpg")
image_stream = io.BytesIO(file.read())
image = Image.open(image_stream)
image.save("bird_original.jpg")

"""
Drawing a rectangle based on the image width and height
"""
width, height = image.size # (width, height)
draw = ImageDraw.Draw(image)
draw.rectangle(((0, 0), (width, height/5)), fill=(241, 244, 66))

"""
Labeling the image, varying the font size depending on the image size
"""
label = "Bird!"
font = ImageFont.truetype("Helvetica", 1)
#print(font.getsize(label)) # (width, height)

while font.getsize(label)[0] < width and font.getsize(label)[1] < height/10:
	font = ImageFont.truetype("Helvetica", font.size+1)

draw.text((width/2-(font.getsize(label)[0]/2), 20), label, font=font,fill=(0, 0, 0))

"""
Saving the picture on disk and previewing it 
"""
image.save("bird_new.jpg")
image.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
