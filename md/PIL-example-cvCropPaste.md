---
title: PIL example cvCropPaste (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'cvCropPaste'


## python cvCropPaste

Python PIL example: cvCropPaste

```python
#crop and paste image
 
from PIL import Image
pil_im = Image.open('./data/empire.jpg').convert('L')
 
#cropping
box = (100,100,400,400)#cropping region
region = pil_im.crop(box)
 
#pasting
paste_pos = (posStartX, posStartY, width, height)
pil_im.paste(region, box)
 
#show
pil_im.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
