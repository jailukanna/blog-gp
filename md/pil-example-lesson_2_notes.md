---
title: pil example lesson 2 notes (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'lesson 2 notes'


Modules used in program: 
* `import numpy as np`

## python lesson 2 notes

Python pil example: lesson 2 notes

```python
import numpy as np
from PIL import Image 
#(pil is pillow module)

i = Image.open('images/dot.png')

iar = np.asarray(i)

print(iar)

'''
install python and pip (as pip contains all the required modules like numpy,matplotlib and pillow or pil)
* start python using terminal and import numpy matplotlib and PIL
* import Images from PIL(run 'from PIL import Image')


So each pixel is measured in RBGA, so an example row is [255, 255, 255, 255], what does that mean?

This means we're looking at a 256-color image, since programming starts with a 0 rather than a 1.
This color means 255 red, 255 green, 255 blue, and then 255 Alpha.

this will print(an array of RGB-A(red,green,blue and alpha )pixels on the screen.)
Alpha is a measure of how opaque an image is. The higher the number, the more solid the color is, 
the lower the number, the more transparent it is.
'''

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
