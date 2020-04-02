---
title: PIL example resize image (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'resize image'


Modules used in program: 
* `import PIL # pip install Pillow`
* `import os`

## python resize image

Python PIL example: resize image

```python
# Python 3.6.7
# OS: Ubuntu 18.04
# Date: 12-14-2018
# Author: Austin Jorgensen
# This batch resizes images to the width you set and keeps aspect ratio. 

import os
import PIL # pip install Pillow
from PIL import Image

base_width = 100        # how many pixels wide to resize to?
target_format = ".png"  # what image format are you targeting?
source_path = "."       # where are you grabbing the original size image?
output_path = "./imgs/" # where do you want to put the resized images?

for files in os.listdir(source_path):
    if files.endswith(target_format):
        img = Image.open(files)
        wpercent = (base_width / float(img.size[0]))
        hsize = int((float(img.size[1]) * float(wpercent)))
        img = img.resize((base_width, hsize), PIL.Image.ANTIALIAS)
        img.save(output_path + files)
        print("Finished: " + files)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
