---
title: PIL example convert to pil (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'convert to pil'


Modules used in program: 
* `import sys`
* `import os`
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python convert to pil

Python PIL example: convert to pil

```python
from PIL import Image
import matplotlib.pyplot as plt
import numpy as np
import os
import sys


root_dir = sys.argv[1]

for file_name in os.listdir(root_dir):
    file_path = os.path.join(root_dir, file_name)
    im = np.array(Image.open(file_path))
    print(im.shape)
    print(np.min(im), np.max(im))
    pil = np.zeros(im.shape[:-1])
    im[im==0] = 0.
    im[im==1] = 255.
    #pil[im[:,:,0] > 200] = 255.
    #plt.imshow(im)
    #plt.savefig(file_path)
    im = Image.fromarray(im)
    im = im.convert('L')
    im.save(file_path)
    print("Saved at :", file_path)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
