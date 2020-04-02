---
title: pil example deepdream (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'deepdream'

Functions in program: 
* `def deepdream(net, base_img, iter_n=10, octave_n=4, octave_scale=1.4,`
* `def make_step(net, step_size=1.5, end='inception_4c/output',`
* `def objective_L2(dst):`
* `def deprocess(net, img):`
* `def preprocess(net, img):`

Modules used in program: 
* `import caffe`
* `import tempfile`
* `import os`
* `import PIL.Image`
* `import scipy.ndimage as nd`
* `import numpy as np`
* `import argparse`

## python deepdream

Python pil example: deepdream

```python
#!/usr/bin/env python2
# based on the code from: https://github.com/google/deepdream
import argparse
parser = argparse.ArgumentParser(description='Deep dreams an image.')
parser.add_argument('image', type=argparse.FileType('rb'), help='image file to process')
parser.add_argument('-g', '--gpu', action='store_true', help='enable GPU acceleration')
parser.add_argument('-o', '--out', type=str, default='out.png', help='output image file')
args = parser.parse_args()

from io import StringIO
import numpy as np
import scipy.ndimage as nd
import PIL.Image
from google.protobuf import text_format
import os
import tempfile

import caffe

if args.gpu:
    caffe.set_mode_gpu()
    # caffe.set_device(0) # select GPU device if multiple devices exist

model_path = os.path.expanduser('~/caffe-rc5/models/bvlc_googlenet/') # substitute your path here
net_fn   = model_path + 'deploy.prototxt'
param_fn = model_path + 'bvlc_googlenet.caffemodel'

# Patching model to be able to compute gradients.
# Note that you can also manually add "force_backward: true" line to "deploy.prototxt".
#model = caffe.io.caffe_pb2.NetParameter()
#print(type(model))
#text_format.Merge(open(net_fn).read(), model)
#model.force_backward = True
#tmp = tempfile.NamedTemporaryFile()
#tmp.write(str(model))
#open('tmp.prototxt', 'w').write(str(model))
#tmp.flush()
#
#net = caffe.Classifier(tmp.name, param_fn,
#                       mean = np.float32([104.0, 116.0, 122.0]), # ImageNet mean, training set dependent
#                       channel_swap = (2,1,0)) # the reference model has channels in BGR order instead of RGB

model = caffe.io.caffe_pb2.NetParameter()
text_format.Merge(open(net_fn).read(), model)
model.force_backward = True
open('tmp.prototxt', 'w').write(str(model))

net = caffe.Classifier('tmp.prototxt', param_fn,
                        mean = np.float32([104.0, 116.0, 122.0]), # ImageNet mean, training set dependent
                        channel_swap = (2,1,0)) # the reference model has channels in BGR order instead of RGB

# a couple of utility functions for converting to and from Caffe's input image layout
def preprocess(net, img):
    return np.float32(np.rollaxis(img, 2)[::-1]) - net.transformer.mean['data']

def deprocess(net, img):
    return np.dstack((img + net.transformer.mean['data'])[::-1])

def objective_L2(dst):
    dst.diff[:] = dst.data

def make_step(net, step_size=1.5, end='inception_4c/output',
              jitter=32, clip=True, objective=objective_L2):
    '''Basic gradient ascent step.'''

    src = net.blobs['data'] # input image is stored in Net's 'data' blob
    dst = net.blobs[end]

    ox, oy = np.random.randint(-jitter, jitter+1, 2)
    src.data[0] = np.roll(np.roll(src.data[0], ox, -1), oy, -2) # apply jitter shift

    net.forward(end=end)
    objective(dst)  # specify the optimization objective
    net.backward(start=end)
    g = src.diff[0]
    # apply normalized ascent step to the input image
    src.data[:] += step_size/np.abs(g).mean() * g

    src.data[0] = np.roll(np.roll(src.data[0], -ox, -1), -oy, -2) # unshift image

    if clip:
        bias = net.transformer.mean['data']
        src.data[:] = np.clip(src.data, -bias, 255-bias)

def deepdream(net, base_img, iter_n=10, octave_n=4, octave_scale=1.4,
              end='inception_4c/output', clip=True, **step_params):
    # prepare base images for all octaves
    octaves = [preprocess(net, base_img)]
    for i in range(octave_n-1):
        octaves.append(nd.zoom(octaves[-1], (1, 1.0/octave_scale,1.0/octave_scale), order=1))

    src = net.blobs['data']
    detail = np.zeros_like(octaves[-1]) # allocate image for network-produced details
    for octave, octave_base in enumerate(octaves[::-1]):
        h, w = octave_base.shape[-2:]
        if octave > 0:
            # upscale details from the previous octave
            h1, w1 = detail.shape[-2:]
            detail = nd.zoom(detail, (1, 1.0*h/h1,1.0*w/w1), order=1)

        src.reshape(1,3,h,w) # resize the network's input image size
        src.data[0] = octave_base+detail
        for i in range(iter_n):
            make_step(net, end=end, clip=clip, **step_params)
            print(octave, i, end, src.data[0].shape)

        # extract details produced on the current octave
        detail = src.data[0]-octave_base
    # returning the resulting image
    return deprocess(net, src.data[0])

img = np.float32(PIL.Image.open(args.image))

result = deepdream(net, img)
PIL.Image.fromarray(np.uint8(result)).save(args.out)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
