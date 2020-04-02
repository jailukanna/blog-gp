---
title: PIL example pil-on-the-fly (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'pil-on-the-fly'

Functions in program: 
* `def interpolate_rectangle2(`
* `def get_circle_image(w):`
* `def create_circular_mask(h, w, center=None, radius=None):`
* `def hello():`
* `def serve_img():`
* `def serve_pil_image(pil_img):`
* `def parse_point(string):`

Modules used in program: 
* `import scipy.interpolate as interpolate`
* `import numpy.linalg as linalg`
* `import numpy as np`

## python pil-on-the-fly

Python PIL example: pil-on-the-fly

```python
#!/usr/bin/env python3

from io import BytesIO
import numpy as np
import numpy.linalg as linalg
import scipy.interpolate as interpolate
from PIL import Image
from flask import Flask, send_file, escape, request

app = Flask(__name__)

def parse_point(string):
  print(string)
  x, y, z = string[1:-1].split(',')
  return int(x), int(y), int(z)

def serve_pil_image(pil_img):
    img_io = BytesIO()
    pil_img.save(img_io, 'JPEG', quality=70)
    img_io.seek(0)
    return send_file(img_io, mimetype='image/jpeg')

@app.route('/route/')
def serve_img():
    
    p0 = np.array(parse_point(request.args.get('p0')))
    p1 = np.array(parse_point(request.args.get('p1')))
    p2 = np.array(parse_point(request.args.get('p2')))
    
    print(p0, p1, p2)
    

    sliceBuffer = interpolate_rectangle2(texture3, p0, p1, p2)
    print(sliceBuffer.shape)
    image = Image.fromarray(sliceBuffer)

    return serve_pil_image(image)
    

@app.route('/')
def hello():
    name = request.args.get("name", "World")
    return f'Hello, {escape(name)}!'

def create_circular_mask(h, w, center=None, radius=None):

    if center is None: 
        raise ValueError('center is not defined')
    if radius is None:
        raise ValueError('radius is not defined')

    Y, X = np.ogrid[:h, :w]
    dist_from_center = np.sqrt((X - center[0])**2 + (Y-center[1])**2)
    mask = dist_from_center <= radius
    return mask

def get_circle_image(w):

  def circle_image(index):  
    angle = np.interp(index, [0, w], [0, np.pi])
    radius = np.interp(np.cos(angle) , [-1, 1], [10, w /2])
    imageBuffer = np.ones([w, w])
    mask = create_circular_mask(w, w, center=[w/2, w/2], radius=radius)
    
    imageBuffer[~mask] = 0
    imageBuffer *= 255

    # image = Image.fromarray(imageBuffer)
    # return image
    return imageBuffer
  return circle_image

def interpolate_rectangle2(
        ct_scans: np.array,
        p0: np.array,
        p1: np.array,
        p2: np.array,
) -> np.array:
    ct_shape = ct_scans.shape
    n = int(np.ceil(linalg.norm(p1 - p0)))  # euclidean distance in pixels
    m = int(np.ceil(linalg.norm(p2 - p0)))  # euclidean distance in pixels
    nu = np.linspace(0, 1, n)[None, :, None]
    mu = np.linspace(0, 1, m)[:, None, None]

    grid = p0[None, None] * (1 - nu - mu) + p1[None, None] * nu + p2[None, None] * mu
    j_points = np.arange(ct_shape[0])
    k_points = np.arange(ct_shape[1])
    i_points = np.arange(ct_shape[2])
    values = interpolate.interpn(
        points=(j_points, k_points, i_points), values=ct_scans, xi=grid, fill_value=0,
        bounds_error=False
    )
    return np.uint8(values * 255)

w = 512
images = list(map(get_circle_image(w), range(w)))
  
if __name__ == '__main__':

  # print((images, len(images)))

  image0 = Image.fromarray(images[0])
  # image0.show()

  image1 = Image.fromarray(images[len(images) // 2])
  # image1.show()

  image2 = Image.fromarray(images[-1])
  # image2.show()

  texture3 = np.stack(images)
  print(texture3.shape)

  p0 = np.array([0, 0, w // 2])
  p1 = np.array([w, 0, w // 2])
  p2 = np.array([0, w, w // 2])

  print(p0, p1, p2)

  sliceBuffer = interpolate_rectangle2(texture3, p0, p1, p2)
  print(sliceBuffer.shape)
  image = Image.fromarray(sliceBuffer)
  image.show()

  app.run()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
