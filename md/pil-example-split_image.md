---
title: pil example split image (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'split image'

Functions in program: 
* `def get_letter_order(im):`
* `def get_unique_colors(img):`

Modules used in program: 
* `import cv2`
* `import numpy as np`
* `import pandas as pd`

## python split image

Python pil example: split image

```python
import pandas as pd
import numpy as np
from PIL import Image
from PIL import ImageEnhance
import cv2

im = Image.open(path)

# Get np.array from image
im_array = np.asarray(im)

# Blur Image
blur = Image.fromarray(cv2.medianBlur(im_array,5))

# Enhance color for easier split
im_ready = ImageEnhance.Color(blur).enhance(100)

# Get unique colors from Image

unique_colors = set()
def get_unique_colors(img):
    for i in range(0,img.size[0]):
        for j in range(0,img.size[1]):
            r,g,b = img.getpixel((i,j))
            unique_colors.add((r,g,b))
    return(unique_colors)

unique_colors = get_unique_colors(im_ready)

# Create a dictionary computing each unique color frequency

from collections import defaultdict
by_color = defaultdict(int)
for pixel in im_ready.getdata():
    by_color[pixel] += 1

# Detect two most frequent colors that are not black 
# (black will always be sorted(by_color.values(),reverse=True)[0]  with over 10k black pixels)

colors_list = []
for e,j in by_color.iteritems():
    if j == sorted(by_color.values(),reverse=True)[1]:
        colors_list.append(e)
    if j == sorted(by_color.values(),reverse=True)[2]:
        colors_list.append(e)

def get_letter_order(im):
    impx = im.load()
    for x in range(0,im.size[0]):
        for y in range(0,im.size[1]):
            if impx[x,y] == colors_list[0]:
                return(colors_list[0],colors_list[1])
            if impx[x,y] == colors_list[1]:
                return(colors_list[1],colors_list[0])

first_letter_color, second_letter_color = get_letter_order(im_ready)

# Get each letter separately by replacing the other color with black pixels

l1 = im_ready.copy()
l2 = im_ready.copy()
im1 = l1.load()
im2 = l2.load()
for x in range(0,im_ready.size[0]):
    for y in range(0,im_ready.size[1]):
        if im_ready.getpixel((x,y)) <> (0,0,0):
            if im1[x,y] <> first_letter_color:
                im1[x,y] = (0,0,0)
            if im2[x,y] <> second_letter_color:
                im2[x,y] = (0,0,0)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
