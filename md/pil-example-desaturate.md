---
title: pil example desaturate (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'desaturate'


## python desaturate

Python pil example: desaturate

```python
#
# You nearly always just need to import Image
# from PIL(low) to do what you need
#
from PIL import Image
#
# It's convenient to import everything from colorzero
# so you don't have to put "colorzero." before everything
#
from colorzero import *

#
# Load an image from the disk into the Pillow Image object
#
image = Image.open("abbey.jpg")

#
# Go across all the columns and down all the rows
# of the image
#
for x in range(image.width):
    for y in range(image.height):
        coord = (x, y)
        #
        # For each coordinate, construct a Color object
        # from the pixel's RGB value
        #
        rgb = image.getpixel(coord)
        pixel = Color(rgb)
        #
        # Apply a 75% saturation to the colour and
        # give the new colour a new name
        #
        desat = pixel * Saturation(0.75)
        #
        # Apply that new colour to the same pixel
        # (you need to use .rgb_bytes to get the
        # 0-255 colours; if you just use .rgb, you'll
        # get RGB values between 0 and 1
        #
        image.putpixel(coord, desat.rgb_bytes)

#
# Save the altered image under a new name
#
image.save("abbey.desat.jpg")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
