---
title: PIL example draw bengali text using pillow (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'draw bengali text using pillow'


Modules used in program: 
* `import time`
* `import cv2`
* `import numpy as np`

## python draw bengali text using pillow

Python PIL example: draw bengali text using pillow

```python
"""
Dependencies:
$sudo apt-get install libfreetype6-dev libharfbuzz-dev libfribidi-dev gtk-doc-tools
$git clone https://github.com/python-pillow/Pillow.git
$cd Pillow/depends
$chmod +x install_raqm.sh
$./install_raqm.sh
$pip install pillow

"""

import numpy as np
from PIL import ImageFont, ImageDraw, Image
import cv2
import time

## Make canvas and set the color
img = np.zeros((200,400,3),np.uint8)
b,g,r,a = 0,255,0,0


## Use bengali font to write bengali.
fontpath = "./Siyamrupali.ttf"  
font = ImageFont.truetype(fontpath, 32)
img_pil = Image.fromarray(img)
draw = ImageDraw.Draw(img_pil)
draw.text((50, 80),  u"দৃষ্টিভঙ্গি", font = font, fill = (b, g, r, a))
img = np.array(img_pil)


cv2.imwrite("res1.png", img)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
