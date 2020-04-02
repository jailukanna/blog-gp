---
title: pil example tfserve (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'tfserve'

Functions in program: 
* `def decode(outputs):`
* `def encode(request_data):`

Modules used in program: 
* `import tensorflow as tf`
* `import tensorflow.contrib.tensorrt as trt`
* `import numpy as np`
* `import tempfile`
* `import PIL`

## python tfserve

Python pil example: tfserve

```python
import PIL
import tempfile
import numpy as np
import tensorflow.contrib.tensorrt as trt
import tensorflow as tf

def encode(request_data):
    with tempfile.NamedTemporaryFile(mode="wb", suffix=".jpg") as f:
        f.write(request_data)
        img = PIL.Image.open(f.name).resize((224, 224))
        img = np.asarray(img) / 255.
    return {"import/input_tensor:0": img}

def decode(outputs):
    p = outputs["import/softmax_tensor:0"]
    index = np.argmax(p)
    return {
               "class": str(index),
               "prob": str(float(p[index]))
           }


from tfserve import TFServeApp
app = TFServeApp("/root/tftrt_int8_resnetv2_imagenet_frozen_graph.pb", ["import/input_tensor:0"],
                 ["import/softmax_tensor:0"], encode, decode)
app.run('0.0.0.0', 5000)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
