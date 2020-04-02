---
title: PIL example image header (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'image header'


Modules used in program: 
* `import urllib2`
* `import random`
* `import os`
* `import math`

## python image header

Python PIL example: image header

```python
# coding: utf-8

import math
import os
import random
import urllib2

# BeautifulSoup: http://www.crummy.com/software/BeautifulSoup/
from bs4 import BeautifulSoup
# PIL: http://www.pythonware.com/products/pil/
from PIL import Image, ImageDraw

# Load the webpage HTML to find all the relevant image URLs
response = urllib2.urlopen('http://www.cs.cornell.edu/people/faculty')
bs = BeautifulSoup(response.read())
tags = bs.findAll('img')
srcs = ['http://www.cs.cornell.edu' + t['src'] for t in tags if t['src'].startswith('/sites/default/files/shared/people/')]


# Where we keep all our images. If you've used this before and are worried
# out-of-date images are around, you should either delete those in particular
# or just delete/empty the directory.
img_dir = 'prof_pics'

# If this is happening for the first time, make a directory for images
if not os.path.exists(img_dir):
    os.mkdir(img_dir)

# Save all the images that aren't already there
for img_url in srcs:
    img_name = img_url.split('/')[-1]
    img_path = os.path.join(img_dir, img_name)
    if img_name.endswith('nophoto.jpg'):
        continue
    if os.path.exists(img_path):
        continue
    with open(img_path, 'wb') as f:
        img = urllib2.urlopen(img_url, img_path).read()
        f.write(img)


# We find all the image file paths and shuffle them later
img_paths = [os.path.join(img_dir, imp) for imp in os.listdir(img_dir)]
colors = ['#fcb508', "#1cd7e8", '#9acb32', '#f08080', '#cc77ee']

# We're fixing 5 rows of images and figuring out the image proportions accordingly
n_rows = 6
img_dim = 100
header_width = 851 * 2
header_height = 315 * 2
n_cols = header_width / img_dim
n_imgs = len(img_paths)
n_spaces = n_cols * n_rows
n_left = n_spaces - n_imgs

img_options = img_paths + random.sample(colors * (n_left / 3), n_left)
random.shuffle(img_options)

# Our blank canvas
header_image = Image.new( 'RGB', (header_width, header_height), "black")

# tile all the images
for i, img_str in enumerate(img_options):
    row = i / n_cols
    col = i % n_cols
    x = (col * img_dim)
    y = row * (img_dim + 6)
    if img_str.startswith('#'):
        draw = ImageDraw.Draw(header_image)
        draw.ellipse((x+4, y+4, x+96, y+96), fill=img_str)
    else:
        img = Image.open(img_str)
        img.thumbnail((img_dim, img_dim), Image.ANTIALIAS)
        header_image.paste(img, (x, y))


# Convert to grayscale and save output image
header_image.save('header_image.png', dpi=(200, 200)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
