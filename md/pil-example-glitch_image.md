---
title: pil example glitch image (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'glitch image'

Functions in program: 
* `def randomize_bytes(data, p_error):`
* `def contrast(image, factor):`
* `def flip_image(image):`
* `def pil_pipeline_step(fn):`

Modules used in program: 
* `import numpy as np`
* `import sys`

## python glitch image

Python pil example: glitch image

```python
import sys
import numpy as np
from PIL import Image
from PIL.ImageEnhance import Contrast
from PIL.ImageOps import invert
from io import BytesIO
from functools import partial
from toolz import compose

def pil_pipeline_step(fn):
    def pipelined_fn(data, *args, **kwargs):
        img = Image.open(BytesIO(data))
        img = fn(img, *args, **kwargs)
        buffer = BytesIO()
        img.save(buffer, format='jpeg')
        return buffer.getvalue()

    return pipelined_fn

@pil_pipeline_step
def flip_image(image):
    image.transpose(Image.FLIP_LEFT_RIGHT)
    return image

@pil_pipeline_step
def contrast(image, factor):
    return Contrast(image).enhance(factor)

invert_colors = pil_pipeline_step(invert)

def randomize_bytes(data, p_error):
    data = bytearray(data)

    n_errors = np.random.binomial(len(data), p_error)
    idxs_errors = np.random.choice(len(data), size=n_errors)

    for idx_error in idxs_errors:
        data[idx_error] = np.random.randint(0, 256)
    
    return bytes(data)

if __name__ == "__main__":
    fname_in = sys.argv[1]
    p_error = float(sys.argv[2])
    fname_out = sys.argv[3]

    with open(fname_in, 'rb') as f:
        data = f.read()

    pipeline = compose(
        flip_image,
        invert_colors,
        partial(randomize_bytes, p_error=p_error),
        invert_colors,
        flip_image,
        partial(contrast, factor=1.8),
        partial(randomize_bytes, p_error=p_error),
    )

    data = pipeline(data)
    
    with open(fname_out, 'wb') as f:
        f.write(data)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
