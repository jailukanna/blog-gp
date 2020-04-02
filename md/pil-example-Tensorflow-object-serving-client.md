---
title: pil example Tensorflow-object-serving-client (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Tensorflow-object-serving-client'


Modules used in program: 
* `import time`
* `import requests`
* `import numpy`
* `import PIL.Image`

## python Tensorflow-object-serving-client

Python pil example: Tensorflow-object-serving-client

```python
import PIL.Image
import numpy
import requests
from pprint(import pprint)
import time

image = PIL.Image.open("dog.jpg")  # Change dog.jpg with your image
image_np = numpy.array(image)


payload = {"instances": [image_np.tolist()]}
start = time.perf_counter()
res = requests.post("http://localhost:8080/v1/models/default:predict", json=payload)
print(f"Took {time.perf_counter()-start:.2f}s")
pprint(res.json())

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
