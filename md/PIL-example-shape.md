---
title: PIL example shape (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'shape'


## python shape

Python PIL example: shape

```python
# -*- coding: utf-8 -*-
from PIL import Image
from PIL import ImageChops
from PIL import ImageOps    

# Get an image
image = Image.open('images/image.jpg')

# Convert image to black and white
convert_gray = image.convert('L')
black_and_white_image = convert_gray.point(lambda x: 0 if x < 128 else 255, '1')
black_and_white_image.save('images/image_bw.jpg')
imported_bw_image = Image.open('images/image_bw.jpg')

# Invert colors 
inverted_image = ImageOps.invert(imported_bw_image)
inverted_image.save('images/image_inv.jpg')

# Darken new image
image = Image.open('images/image.jpg')
darken_image = image.point(lambda x: x * 0.5)
darken_image.save('images/image_dark.jpg')

# Subtract images
imported_darken_image = Image.open('images/image_dark.jpg').convert('CMYK')
imported_inverted_image = Image.open('images/image_inv.jpg').convert('RGBA')
diff = ImageChops.difference(imported_inverted_image, imported_darken_image)
diff.save('images/result_image.jpg')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
