---
title: pil example pyfont (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'pyfont'


Modules used in program: 
* `import numpy as np`
* `import PIL`

## python pyfont

Python pil example: pyfont

```python
from os import listdir
from os.path import isfile, join
path = "c:\\windows\\fonts"
onlyfiles = [f for f in listdir(path) if isfile(join(path, f))]
print(onlyfiles)

%matplotlib inline
import PIL
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw
import numpy as np
from skimage import exposure
from matplotlib import pyplot as plt

font = ImageFont.truetype(join(path, "ITCKRIST.ttf"),30)
img = Image.new("RGBA", (30,30),(255,255,255))
draw = ImageDraw.Draw(img)

draw = draw.text((0, 0),"Aa",(0,0,0),font=font)
draw = ImageDraw.Draw(img)

plt.imshow(np.asarray(img))
img.save('d:\\test.jpg')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
