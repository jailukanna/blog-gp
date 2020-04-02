---
title: pil example mask rcnn benchmark (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'mask rcnn benchmark'

Functions in program: 
* `def fun():`
* `def load(url):`

Modules used in program: 
* `import torch`
* `import timeit`
* `import numpy as np`
* `import requests`

## python mask rcnn benchmark

Python pil example: mask rcnn benchmark

```python
import requests
from io import BytesIO
from PIL import Image
import numpy as np
import timeit
import torch

from maskrcnn_benchmark.config import cfg
from predictor import COCODemo, to_image_list

config_file = "../configs/caffe2/e2e_mask_rcnn_R_50_FPN_1x_caffe2.yaml"

# update the config options with the config file
cfg.merge_from_file(config_file)
# manual override some options
#cfg.merge_from_list(["MODEL.DEVICE", "cpu"])

coco_demo = COCODemo(
    cfg,
    min_image_size=1024,
    confidence_threshold=0.7,
)

def load(url):
    """
    Given an url of an image, downloads the image and
    returns a PIL image
    """
    response = requests.get(url)
    pil_image = Image.open(BytesIO(response.content)).convert("RGB")
    # convert to BGR format
    image = np.array(pil_image)[:, :, [2, 1, 0]]
    return image

image = load("http://farm3.staticflickr.com/2469/3915380994_2e611b1779_z.jpg")
image_list = to_image_list(coco_demo.transforms(image), coco_demo.cfg.DATALOADER.SIZE_DIVISIBILITY).to('cuda')

def fun():
    with torch.no_grad():
        coco_demo.model(image_list)

for i in range(10):
    fun()

print(timeit.timeit(fun, number=100))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
