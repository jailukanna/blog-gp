---
title: pil example grey histogram (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'grey histogram'


Modules used in program: 
* `import math`
* `import matplotlib.pyplot as plt`

## python grey histogram

Python pil example: grey histogram

```python
from PIL import Image
import matplotlib.pyplot as plt
import math
# image on which histogram is to be plotted 
img = Image.open("greycat.jpg") 
pixels = img.load()
pixelMap = {}
for i in range(img.size[0]):
    for j in range(img.size[1]):
      val = pixels[i, j]
      if pixelMap.get(val) == None:    #just get the pixel intensity of the grey scale image
        pixelMap[val] = 1
      else:
        pixelMap[val] = pixelMap[val]+1
plt.bar(list(pixelMap.keys()), pixelMap.values(), color='black')
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
