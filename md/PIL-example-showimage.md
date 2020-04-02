---
title: PIL example showimage (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'showimage'


Modules used in program: 
* `import webbrowser`
* `import webbrowser`
* `import time`
* `import webbrowser`
* `import time`

## python showimage

Python PIL example: showimage

```python
from PIL import Image
import time
grayIm = Image.new('L', (28, 28))
grayIm.putdata([0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,41,149,156,179,254,254,201,119,46,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,13,147,241,253,253,254,253,253,253,253,245,160,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,31,224,253,253,180,174,175,174,174,174,174,223,247,145,6,0,0,0,0,0,0,0,0,0,0,0,0,7,197,254,253,165,2,0,0,0,0,0,0,12,102,184,16,0,0,0,0,0,0,0,0,0,0,0,0,152,253,254,162,18,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,235,254,158,15,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,74,250,253,15,0,0,0,16,20,19,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,7,199,253,253,0,0,25,130,235,254,247,145,6,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,20,253,253,177,100,219,240,253,253,254,253,253,125,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,5,193,253,253,254,253,253,200,155,155,238,253,229,23,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,61,249,254,241,150,30,0,0,0,215,254,254,58,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,36,39,30,0,0,0,0,0,214,253,234,31,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,41,241,253,183,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,201,253,253,102,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,114,254,253,154,5,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,62,254,255,241,30,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,10,118,235,253,249,103,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,55,81,0,102,211,253,253,253,135,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,79,243,234,254,253,253,216,117,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,48,245,253,254,207,126,27,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0])
# grayIm.format='BMP'
# grayIm.show()
grayIm.save("test.jpg")
import webbrowser
webbrowser.open("test.jpg")
print(grayIm.format, grayIm.size, grayIm.mode)

from PIL import Image
import time
img = Image.new( 'RGB', (255,255), "black") # create a new black image
pixels = img.load() # create the pixel map

for i in range(img.size[0]):    # for every pixel:
    for j in range(img.size[1]):
        pixels[i,j] = (i, j, 100) # set the colour accordingly

# img.show()
img.save("test.jpg")
import webbrowser
webbrowser.open("test.jpg")
# time.sleep(30)

from PIL import Image, ImageDraw
from random import randint
picture = Image.new("RGB", (600, 600))
artist = ImageDraw.Draw(picture)
for i in range(100):
    x1, y1 = randint(0, 600), randint(0, 600)
    x2, y2 = randint(0,600), randint(0,600)
    color = (randint(0, 255), randint(0, 255), randint(0, 255))
    width = randint(2, 20)
    artist.line([x1, y1, x2, y2], color, width)
#picture.convert("RGB")
#picture.show()
# save the image to show it with the web browser's viewer
filename = "aa_image.jpg"
picture.save(filename)
import webbrowser
webbrowser.open(filename)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
