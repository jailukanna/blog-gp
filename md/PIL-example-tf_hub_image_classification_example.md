---
title: PIL example tf hub image classification example (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'tf hub image classification example'

Functions in program: 
* `def download_and_resize_image(url, filename, new_width=256, new_height=256):`

Modules used in program: 
* `import cv2`

## python tf hub image classification example

Python PIL example: tf hub image classification example

```python
from urllib.request import urlopen
from io import BytesIO
from PIL import Image, ImageOps
import cv2

def download_and_resize_image(url, filename, new_width=256, new_height=256):
  response = urlopen(url)
  image_data = response.read()
  image_data = BytesIO(image_data)
  pil_image = Image.open(image_data)
  pil_image = ImageOps.fit(pil_image, (new_width, new_height), Image.ANTIALIAS)
  pil_image_rgb = pil_image.convert('RGB')
  pil_image_rgb.save(filename, format='JPEG', quality=90)
# instantiating module 
tf.reset_default_graph()
module = hub.Module("https://tfhub.dev/google/imagenet/inception_v3/classification/1")
height, width = hub.get_expected_image_size(module)

img_url = "https://cdn.pixabay.com/photo/2017/02/20/18/03/cat-2083492_960_720.jpg"
img_jpg = "cat.jpg"
​
# input and output tensor
input_tensor = tf.placeholder(tf.float32, shape=[1, width, height, 3])
logits = module(input_tensor)
labels = tf.argmax(logits, axis=1)

# download_and_resize_image(img_url, img_jpg, width, height)
image = cv2.imread(img_jpg)
# input range from 0-1
image_rgb = (cv2.cvtColor(image, cv2.COLOR_BGR2RGB)/256)
inputs = np.expand_dims(image_rgb, 0).astype(np.float32)

# set up session
initializer = tf.global_variables_initializer()
sess = tf.Session()
sess.run(initializer)

# predict
res = sess.run(labels, feed_dict={input_tensor: inputs})
print(res)

# 283: 'Persian cat',

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
