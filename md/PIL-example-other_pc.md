---
title: PIL example other pc (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'other pc'


Modules used in program: 
* `import numpy as np`
* `import cv2`
* `import numpy as np`

## python other pc

Python PIL example: other pc

```python
import numpy as np
from PIL import ImageGrab
from PIL import Image
import cv2
import numpy as np

resim = ImageGrab.grab(bbox=(331, 168, 481, 201))

resim_array = np.array(resim)



cv2.imshow("win", resim_array)






```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
