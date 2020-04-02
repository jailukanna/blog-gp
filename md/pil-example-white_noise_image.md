---
title: pil example white noise image (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'white noise image'

Functions in program: 
* `def get_white_noise_image(width, height):`

## python white noise image

Python pil example: white noise image

```python
from PIL import Image

def get_white_noise_image(width, height):
    pil_map = Image.new("RGBA", (width, height), 255)
    random_grid = map(lambda x: (
            int(random.random() * 256),
            int(random.random() * 256),
            int(random.random() * 256)
        ), [0] * width * height)
    pil_map.putdata(random_grid)
    return pil_map

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
