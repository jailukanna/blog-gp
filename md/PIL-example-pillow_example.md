---
title: PIL example pillow example (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'pillow example'

Functions in program: 
* `def show_image(fname):    `

Modules used in program: 
* `import numpy as np`
* `import numpy as np`

## python pillow example

Python PIL example: pillow example

```python
from matplotlib.pyplot import imshow
import numpy as np
from PIL import Image

%matplotlib inline
pil_im = Image.open('/mnt/data1/images/correct/0.jpg', 'r')
imshow(np.asarray(pil_im))

# 함수로 사용하면 편리하다
%matplotlib inline
from matplotlib.pyplot import imshow
import numpy as np
from PIL import Image
    
def show_image(fname):    
    pil_im = Image.open(fname, 'r')
    imshow(np.asarray(pil_im))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
