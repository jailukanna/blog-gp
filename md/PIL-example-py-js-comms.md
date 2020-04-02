---
title: PIL example py-js-comms (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'py-js-comms'

Functions in program: 
* `def run_algo(imgB64):`
* `def data_uri_to_img(uri):`

Modules used in program: 
* `import logging`
* `import base64`
* `import cv2`
* `import numpy as np`
* `import sys`
* `import time`
* `import IPython`

## python py-js-comms

Python PIL example: py-js-comms

```python
import IPython
import time
import sys
import numpy as np
import cv2
import base64
import logging

from google.colab import output
from PIL import Image
from io import BytesIO

def data_uri_to_img(uri):
    """convert base64 image to numpy array"""
    try:
        image = base64.b64decode(uri.split(',')[1], validate=True)
        # make the binary image, a PIL image
        image = Image.open(BytesIO(image))
        # convert to numpy array
        image = np.array(image, dtype=np.uint8); 
        return image
    except Exception as e:
        logging.exception(e);print('\n')
        return None

def run_algo(imgB64):
    """
    in Colab, run_algo function gets invoked by the JavaScript, 
    that sends N images every second, one at a time.
    params:
        image: image
    """
    image = data_uri_to_img(imgB64)  
    if image is None:
        print("At run_algo(): image is None.")
        return

    try:
        # Run detection
        results = model.detect([image], verbose=1)
        # Visualize results
        r = results[0]    
        visualize.display_instances(
            image, 
            r['rois'], 
            r['masks'], 
            r['class_ids'], 
            class_names, 
            r['scores']
            )
    except Exception as e:
        logging.exception(e)
        print('\n')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
