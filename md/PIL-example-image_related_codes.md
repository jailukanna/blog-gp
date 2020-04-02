---
title: PIL example image related codes (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'image related codes'


Modules used in program: 
* `import numpy as np`
* `import numpy, Image`
* `import requests`

## python image related codes

Python PIL example: image related codes

```python
from PIL import Image
import requests
from io import BytesIO

response = requests.get(url)
img = Image.open(BytesIO(response.content))

im = Image.open(requests.get(url, stream=True).raw)

import numpy, Image

i = Image.open('lena.jpg')
a = numpy.asarray(i) # a is readonly
i = Image.fromarray(a)

>>> I = numpy.asarray(PIL.Image.open('test.jpg'))

>>> im = PIL.Image.fromarray(numpy.uint8(I))


from PIL import Image
import numpy as np

w, h = 512, 512
data = np.zeros((h, w, 3), dtype=np.uint8)
data[256, 256] = [255, 0, 0]
img = Image.fromarray(data, 'RGB')
img.save('my.png')
img.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
