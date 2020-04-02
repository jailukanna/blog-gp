---
title: pil example url2im2arr (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'url2im2arr'

Functions in program: 
* `def url2arr(url, requests_kwargs=None, pil_kwargs=None):`

Modules used in program: 
* `import requests`

## python url2im2arr

Python pil example: url2im2arr

```python
import requests
from PIL import Image
from io import BytesIO

# the one-liner
np.array(Image.open(BytesIO(requests.get('http://not.another/url.png', stream=True).content)).convert('L'))


def url2arr(url, requests_kwargs=None, pil_kwargs=None):
    response = requests.get(url, stream=True, **(requests_kwargs or {}))
    response.raise_for_status()
    buffer = BytesIO(response.content)  # opening directly from raw response doesn't work for JPEGs
    raw_img = Image.open(buffer)
    grey_img = raw_img.convert('L', **(pil_kwargs or {}))  # takes care of alphas, squashes colour channels
    return np.array(grey_img)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
