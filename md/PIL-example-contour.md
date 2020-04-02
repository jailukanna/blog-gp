---
title: PIL example contour (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'contour'


## python contour

Python PIL example: contour

```python
# -*- coding: utf-8 -*-
from PIL import Image
from PIL import ImageFilter


# Get an image
image = Image.open('images/image.jpg')

# Get the contour
image = image.filter(ImageFilter.CONTOUR)

# Save image
image.save('images/image_contour.jpg')

# Show image
image.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
