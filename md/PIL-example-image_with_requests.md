---
title: PIL example image with requests (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'image with requests'


Modules used in program: 
* `import numpy as np`
* `import requests`

## python image with requests

Python PIL example: image with requests

```python
import requests
from PIL import Image
from io import BytesIO
import numpy as np

res = requests.get(image_url)
image = Image.open(BytesIO(res.content))
image_array = np.array(image)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
